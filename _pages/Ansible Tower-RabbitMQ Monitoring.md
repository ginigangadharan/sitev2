
## Ansible Tower: RabbitMQ Monitoring


Is it Possible to Monitor RabbitMQ for Ansible Tower?
Yes! RabbitMQ allows the ability to monitor through a plugin called RabbitMQ Manager.
RabbitMQ Manager allows users to view RabbitMQ message queues in real time through a UI implementation as well as through its own API interface.
Please note that RabbitMQ was introduced in Ansible Tower 3.1. This guide is only useful for versions either equal to or greater than 3.1.

General RabbitMQ Configurations for Ansible Tower
Ports:

4369
25672
15672 (with RabbitMQ Manager enabled, see more below)
Partitioning Strategy: pause_minority

For more details, please see Ansible Tower's cluster setup considerations

How do I setup RabbitMQ Manager?
You can either pass the extra variable to the setup.sh installer:

./setup.sh -e rabbitmq_enable_manager=true

Or add the variable under [all:vars] in inventory:

Raw
[all:vars]
admin_password='redact'

rabbitmq_enable_manager=true

pg_host='10.10.2.20'
pg_port='5432'
The above can be done during a fresh install, upgrade to a 3.1+ version, or after an install. Our installer playbooks are idempotent and the above changes will only modify the state of the enabled plugins for RabbitMQ. Please note that this will require a small window of downtime.

Once the installer is complete, the RabbitMQ Manager interface is available on all [tower] nodes at the following addresses:

UI: http://nodeN.tower.example.org:15672
API: http://nodeN.tower.example.org:15672/api
**Where: nodeN.tower.example.org is the Nth node in your cluster (IP or FQDN will work here as long as they're routable to the user)
Now that the RabbitMQ Manager is Setup, What Should I Monitor?
You can find the health of the node with a GET to the following endpoint:
http://nodeN.tower.example.org:15672/api/healthchecks/node/

If the node is healthy, we expect a 200 status along with the following message:

Raw
{"status":"ok"}
Another helpful metric to follow is message count. We do not expect a backlog of messages in Regular Tower operations. Here's an API query using jq:
curl -s -u tower http://nodeN.tower.example.org:15672/api/queues/tower/tower/ | jq .messages
**An idle Tower node should return 0

To verify that the queues are in a healthy state when troubleshooting an issue live, you can check the tasks directly from command line as root:
For Ansible Tower >= v3.3: awx-manage celery inspect active
For Ansible Tower >= v3.4: awx-manage run_dispatcher --status
rabbitmqctl cluster_status
You can also see messages published and consumed in the RabbitMQ UI for each node.
Both the above and the UI are helpful to monitor message delivery issues in real time.

Also monitoring for get/deliver no_ack can provide further insight into RabbitMQ Activity for prolonged monitoring:
curl -s -u tower http://nodeN.tower.example.org:15672/api/queues/tower/tower/ | jq .message_stats.deliver_no_ack
curl -s -u tower http://nodeN.tower.example.org:15672/api/queues/tower/tower/ | jq .message_stats.get_no_ack

These are just a few examples. Please take a look at the following documentation on how to expand monitoring further through the RabbitMQ Manager plugin:
RabbitMQ Management HTTP API
RabbitMQ Management HTTP Stats

Are There Any Logs to Check on the Filesystem?
The RabbitMQ logs can be found under /var/log/rabbitmq/$NODENAME.log where generally $NODENAME=rabbitmq@rabbitmq_host (rabbitmq_host = inventory_hostname unless otherwise specified)

When to Monitor?
RabbitMQ issues will always result in a node within the Tower cluster to show zero capacity. If you consistently see issues with capacity after multiple RabbitMQ recoveries/restarts, then you may want to enable monitoring. You can view per node capacity either through the UI at Settings > Instance Groups or through the API endpoint /api/v2/ping/.

Common Errors
Raw
=INFO REPORT==== DD-Mon-YYYY::HR:Mi:SS ===
RabbitMQ is asked to stop...
SIGTERM either through expected service interruption or partitioning due to a high volume of messages

Raw
=INFO REPORT==== DD-Mon-YYYY::HR:Mi:SS ===
Mirrored queue 'asgi:tower:websocket:websocket.send.CELERY_TASK_HASH_HERE' in vhost 'tower': Slave <rabbitmq@rabbitmq_host> saw deaths of mirrors <rabbitmq@other_rabbitmq_host>
The current clustered node is observing issues with another clustered node. This error doesn't directly indicate an issue with the current clustered node, but a good indicator to investigate the other cluster node.

Raw
=ERROR REPORT==== DD-Mon-YYYY::HR:Mi:SS ===
Mnesia('rabbitmq@{{rabbitmq_host}}'): ** ERROR ** mnesia_event got {inconsistent_database, running_partitioned_network, 'rabbitmq@{{other_rabbitmq_host}}'}
Likely a partitioning issue

Raw
=ERROR REPORT==== DD-Mon-YYYY::HR:Mi:SS ===
** Node 'rabbitmq@{{other_rabbitmq_host}}' not responding **
** Removing (timedout) connection **
Connection issue to problem node as seen from a stable node

Raw
=WARNING REPORT==== DD-Mon-YYYY::HR:Mi:SS ===
Mirrored queue '{{rabbitmq_host}}' in vhost '{{rabbitmq_vhost}}': Stopping all nodes on master shutdown since no synchronised slave is available
This is a connection issue as seen from the problem node. By default, Tower's partitioning strategy is set to pause_minority. This is a good indication of a minority node being shutdown.