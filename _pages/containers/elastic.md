

## Check elastic connectivity

From same pod or from another test pod, test the details.

```shell
[ root@curl:/ ]$ curl -XGET elasticsearch:9200
{
  "name" : "es-cluster-0",
  "cluster_name" : "k8s-logs",
  "cluster_uuid" : "SA35OAyAR_GLhFfvUvOkkw",
  "version" : {
    "number" : "7.5.0",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "e9ccaed468e2fac2275a3761849cbee64b39519f",
    "build_date" : "2019-11-26T01:06:52.518245Z",
    "build_snapshot" : false,
    "lucene_version" : "8.3.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

*Note: Run `kubectl run curl --image=radial/busyboxplus:curl -i --tty` to run a test pod with shell.*

## References

[Release Notes](https://www.elastic.co/guide/en/elasticsearch/reference/current/es-release-notes.html)
[How to Setup EFK Stack on Kubernetes: Step by Step Guides](https://devopscube.com/setup-efk-stack-on-kubernetes/) (devopscube)