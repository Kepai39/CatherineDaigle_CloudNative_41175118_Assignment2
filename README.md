# CatherineDaigle_CloudNative_41175118_Assignment2
 Its the main repository that lists all the files and workflow


## Setting up AKS

### Getting the Kube config
     ```cat ~/.kube/config | base64 -b 0 > kube_config_base64.txt``` (-b is for mac, use -w for windows)

# Dockerizing each Service





# Deploying AI
Access the Azure OpenAI Resource:

Navigate to the Azure OpenAI resource you just created.
Deploy GPT-4:

Go to the Model Deployments section and click Add Deployment.
Choose GPT-4 as the model and provide a deployment name (e.g., gpt-4-deployment).
Set the deployment configuration as required and deploy the model.
Deploy DALL-E 3:

Repeat the same process to deploy DALL-E 3.
Use a descriptive deployment name (e.g., dalle-3-deployment).
Note Configuration Details:

Once deployed, note down the following details for each model:
Deployment Name
Endpoint URL