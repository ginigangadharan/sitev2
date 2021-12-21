#Here are the links for getting Public IPv4 on various clouds.

Azure

curl -H Metadata:true "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-08-01&format=text"

AWS

curl http://169.254.169.254/latest/meta-data/public-ipv4

GCP

curl -H "Metadata-Flavor: Google" http://169.254.169.254/computeMetadata/v1/instance/network-interfaces/0/access-configs/0/external-ip

Since 169.254.169.254 is a global metadata server for these above cloud vendors, no need to use different domain names.