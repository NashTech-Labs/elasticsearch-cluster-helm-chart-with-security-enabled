# elasticsearch-cluster-helm-chart-with-security-enabled
## Elasticsearch with xpack-security enabled

Elasticsearch is a distributed, free and open search and analytics engine for all types of data, including textual, numerical, geospatial, structured, and unstructured.
This heln chart will deploy elastic search with security configured and the inter node connection is secured using self signed ssl certifactes.

#### prerequisites

1. Cert-manager: It is used to issue self-signed certificate using custom CA.

## Need to change 

You need to change the following things in values.yaml only:
1. commonName
2. OrganizationName

3. now, deploy the helm chart on the kubernetes cluster and wait for the cluster to come in the running state.
4. Once the cluster is in running state then exec any of the pod of elasticsearch and run the following command to generate passwords:

    bin/elasticsearch-setup-passwords auto|interactive

5. The passwords will look like this: 

      Changed password for user apm_system
      PASSWORD apm_system = 6mvXw9UFiuEsrAtascRf

      Changed password for user kibana_system
      PASSWORD kibana_system = xzzhgscTNBpaFjnVRedf

      Changed password for user kibana
      PASSWORD kibana = xzzhgscTNBpaFjnVICdgr

      Changed password for user logstash_system
      PASSWORD logstash_system = Zwyh4Jzw5d1XR4Fhhtg

      Changed password for user beats_system
      PASSWORD beats_system = c74xUdNMCYWSB7ajbtbq

      Changed password for user remote_monitoring_user
      PASSWORD remote_monitoring_user = YBdDggF2Mhk40PTErd3r

      Changed password for user elastic
      PASSWORD elastic = Dj7t68XaPXKP89ROuy5f

6. Once the password generated copy it and place it some safe place because you need them later for the authentication.
7. Take the password for elastic and add it to secret.yaml by converting it to base64.
8. Redploy the elasticsearch cluster using command:

    helm upgrade -i elasticsearch elasticsearch/

9. Now, your cluster comming to green state and is secure, to test this run following command:

    curl elasticsearch-master:9200/_cluster/health?pretty

    It will not able to authenticate and return status code 403 unauthorized

10. Now use username and password:

    curl -u username:password elasticsearch-master:9200/_cluster/health?pretty

Now your elasticsearch has security enabled and is also enabled ssl security for inter-node connection.