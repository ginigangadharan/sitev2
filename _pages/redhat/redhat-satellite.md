---
layout: post
title: Red Hat Satellite Server
author: gini
categories: [ redhat ]
image: "assets/images/2020/infrastructure.jpg"
tags: [infrastructure, rhel]
permalink: satellite-server
featured: false
hidden: false
#titleshort: satellite-server
---


# Reference Documents
- [Red Hat Satellite Quick Start](https://access.redhat.com/documentation/en-us/red_hat_satellite/6.7/html-single/quick_start_guide/index) - Installing, configuring, and provisioning physical and virtual hosts from Red Hat Satellite Servers.
- [INSTALLING SATELLITE SERVER](https://access.redhat.com/documentation/en-us/red_hat_satellite/6.7/html/quick_start_guide/installing_satellite_server)
- [ASSOCIATING OBJECTS WITH THE DEFAULT ORGANIZATION AND LOCATION](https://access.redhat.com/documentation/en-us/red_hat_satellite/6.7/html/quick_start_guide/associating_objects_with_the_default_organization_and_location)
- [IMPORTING SUBSCRIPTIONS AND SYNCHRONIZING CONTENT](https://access.redhat.com/documentation/en-us/red_hat_satellite/6.7/html/quick_start_guide/importing_subscriptions_and_synchronizing_content)
- [MANAGING AND PROMOTING CONTENT](https://access.redhat.com/documentation/en-us/red_hat_satellite/6.7/html/quick_start_guide/managing_and_promoting_content)
- [PATCHING YOUR SYSTEMS](https://access.redhat.com/documentation/en-us/red_hat_satellite/6.7/html/quick_start_guide/patching_your_systems)


After installing Satellite with the satellite-installer command, all Satellite services are started and enabled automatically. View the list of these services by executing:

```shell
# satellite-maintain service list
To see the status of running services, execute:

# satellite-maintain service status
To stop the satellite-maintain services, execute:

# satellite-maintain service stop
To start the satellite-maintain services, execute:

# satellite-maintain service start
To restart the satellite-maintain services, execute:

# satellite-maintain service restart
```

Refer: [Starting and stopping red hat satellite](https://access.redhat.com/documentation/en-us/red_hat_satellite/6.9/html/administering_red_hat_satellite/chap-red_hat_satellite-administering_red_hat_satellite-starting_and_stopping_red_hat_satellite)