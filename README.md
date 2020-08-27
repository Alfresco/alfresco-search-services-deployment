# Alfresco Search Deployment

**NOTE: This repository is now deprecated and archived as the Search Helm Chart has been moved within the ACS Helm Chart, you can now find it here: https://github.com/Alfresco/acs-deployment/tree/master/helm/alfresco-content-services/charts/alfresco-search.**

## Contents of this repository
a) Alfresco Content Services deployed via [docker-compose](docker-compose/docker-compose.yml)
1. Alfresco Repository
2. Alfresco Share
3. A Postgres DB  
4. Alfresco Insight Engine (Quay.io access is required. Please [contact support](http://support.alfresco.com/) if you don’t yet have it)
5. Alfresco Insight Zeppelin (Quay.io access is required. Please [contact support](http://support.alfresco.com/) if you don’t yet have it)

b) Alfresco Search ([alfresco-search](./helm/alfresco-search)) helm charts used as a requirement under [acs-deployment/helm/alfresco-content-services](https://github.com/Alfresco/acs-deployment/tree/master/helm/alfresco-content-services) Chart

>:exclamation: Search Services will be deployed as part of ACS and it is defined as a requirement in [ACS helm chart](https://github.com/Alfresco/acs-deployment/tree/master/helm/alfresco-content-services).

## Alfresco Search - Helm Chart

### Deployment options
* [Deploying with Helm charts on AWS using Kops](https://github.com/Alfresco/acs-deployment/tree/master/docs/helm-deployment-aws_kops.md)
* [Deploying with Helm charts on AWS using EKS](https://github.com/Alfresco/acs-deployment/tree/master/docs/helm-deployment-aws_eks.md)

### Configurations

The following table lists the configurable parameters of the [Alfresco Search](./helm/alfresco-search) chart and their default values. 

>For Alfresco Content Service chart please consult [this table](https://github.com/Alfresco/acs-deployment/blob/master/helm/alfresco-content-services/README.md#configuration).

Parameter | Description | Default
--- | --- | ---
`alfresco-search.type` | Define the type of Alfresco Search to use. Available options: `insight-engine` or `search-services` | `search-services`
`alfresco-search.global.alfrescoRegistryPullSecrets` | This property does not have to be set if `alfresco-search.type` is not set to `insight-engine` explicitly as the Docker image in [DockerHub](https://hub.docker.com/r/alfresco/alfresco-search-services/tags/) is publicly available| NONE
`alfresco-search.ingress.enabled` | Enable external access for Alfresco Search Services | `true`
`alfresco-search.ingress.basicAuth` | if `alfresco-search.ingress.enabled` is `true`, user needs to provide a `base64` encoded `htpasswd` format user name & password (ex: `echo -n "$(htpasswd -nbm solradmin somepassword)"` where `solradmin` is username and `somepassword` is the password) | NONE
`alfresco-search.ingress.whitelist_ips` | if `ingress.enabled=true`, user can restrict /solr to a list of IP addresses of CIDR notation | `0.0.0.0/0`
`alfresco-search.alfresco-insight-zeppelin.enabled` | Enabled Alfresco Insight Zeppelin (_this will work only with InsightEngine image_) | `false`

# Search type - insight-engine

If you choose search type insight-engine during AWS EKS deployment then we need to add the below parameter in helm install command

`--set alfresco-search.environment.SOLR_OPTS="-Dsolr.content.dir=/opt/alfresco-insight-engine/data/contentstore" --set alfresco-search.persistence.search.data.mountPath="/opt/alfresco-insight-engine/data"`

This parameter helps to mount EBS volume in the correct location. By default its /opt/alfresco-search-service/data/

## Example command: 
`helm install alfresco-stable/alfresco-content-services --version 2.0.0 --name acs --namespace=$DESIREDNAMESPACE --set alfresco-search.type=insight-engine --set alfresco-search.environment.SOLR_OPTS="-Dsolr.content.dir=/opt/alfresco-insight-engine/data/contentstore" --set alfresco-search.persistence.search.data.mountPath="/opt/alfresco-insight-engine/data"`

helm install alfresco-stable/alfresco-content-services --version
## Contributing guide
Please use [this guide](CONTRIBUTING.md) to make a contribution to the project and information to report any issues.

## Other Information
* Checkout [acs-deployment](https://github.com/Alfresco/acs-deployment/blob/master/README.md#other-information) readme.
