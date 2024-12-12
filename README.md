# CatherineDaigle_CloudNative_41175118_Assignment2
 Its the main repository that lists all the files and workflow




# Deployment Instructions:

## Setting up AKS
Step 1: Create AKS
- Region: Canada Central
- AKS Pricing: Free
- Automatic Upgrade: Disabled
- Node Security Channel Type: None
- Authentication and Authorization: Local accounts with Kubernetes RBAC

Step 2: Create Nodes:
- masterpool node, D2as V4 with Manual scaling set to 1 Node
- workerpool node, mode: user, D2as V4 with manual scaling set to 1 node (saving costs)

Step 3: Review & Create

### Getting the Kube config
     ```cat ~/.kube/config | base64 -b 0 > kube_config_base64.txt``` (-b is for mac, use -w for windows)

## Setting up Azure Service Bus:
Step1: using Azure CLI  create the service bus:
```
az login
az servicebus namespace create --name A2ServiceBus --resource-group CloudNativeA2
az servicebus queue create --name orders --namespace-name A2ServiceBus --resource-group CloudNativeA2
Pick Microsoft Entra Identity Workload
```

Step2: Assign azure service bus sender:
```
PRINCIPALID=$(az ad signed-in-user show --query objectId -o tsv)
SERVICEBUSBID=$(az servicebus namespace show --name A2ServiceBus --resource-group CloudNativeA2 --query id -o tsv)
az role assignment create --role "Azure Service Bus Data Sender" --assignee $PRINCIPALID --scope $SERVICEBUSBID

```

Step 3: Get Connection Info:
```
HOSTNAME=$(az servicebus namespace show --name A2ServiceBus --resource-group CloudNativeA2 --query serviceBusEndpoint -o tsv | sed 's/https:\/\///;s/:443\///')
```




# Deploying AI
Step 1: Create OpenAI Resource within East US, Standard 0$.
Step 2: go to AI Foundry
Step 3: under deployments, click add and choose GPT-4 as the deployment. (EastUS)
Step 4: under deployments, click add and choose DALL-E3 as the deployment.(EastUS)
Step 5: Copy these details for each deployment:
Deployment Name
Endpoint URL

Step 6: Enter the values under the AI service within the aps-all-in-one.yaml file.
- name: AZURE_OPENAI_API_VERSION
  value: "2024-07-01-preview"
- name: AZURE_OPENAI_DEPLOYMENT_NAME
  value: "gpt-4-deployment"
- name: AZURE_OPENAI_ENDPOINT
  value: "https://<your-openai-resource-name>.openai.azure.com/"
- name: AZURE_OPENAI_DALLE_ENDPOINT
  value: "https://<your-openai-resource-name>.openai.azure.com/"
- name: AZURE_OPENAI_DALLE_DEPLOYMENT_NAME
  value: "dalle-3-deployment"

Step 7: in terminal input the following when kubernetes deployed:
  kubectl apply -f config-maps.yaml
  kubectl apply -f secrets.yaml
  kubectl get configmaps
  kubectl get secrets



  ## How to deploy into Kubernetes:
  kubectl apply -f aps-all-in-one.yaml