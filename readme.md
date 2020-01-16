# Retrieve versioned secrets from Keyvault using Azure Pipeline YAML 

This yaml file can be used for Azure Pipelines to pull secrets from Keyvault. 
In this case it 
1. pulls a secret
2. sends the secret to httpmirror to display its value to prove that it worked as the value of a secret will always be hidden in Azure pipelines (therefore you can not display it using echo or Write-Host).

# Flow
1. Setup the pipeline in Azure DevOps using the yaml
2. Create a separate branch and modify the yaml to pull another version of the secret.
3. Create a PR to master. This will trigger the build.
4. During the build the value of the secret will be sent to httpmirror.
5. Open the website of your httpmirror to display the current value.
6. The value of the secret is either "blue", "red" or "green" depending on the version you pick.

# Setup 
Secrets in Keyvault are versioned like this: secretname/guid-for-version. 

To retrieve them use the corresponding key vault task in Azure pipelines. Make sure the service principal used has access policies set on the secrets (Get/List) and  has Azure RBAC role READER on the keyvault.

# Background
If you want to make sure that an update of a secret does not accidentally break your application, you should consider to reference a versioned secret and merge an update to master via PR which allows for a pre-check. So you decide in code when to update to a new version of the secret and get all the review & testing capabilities your pipeline offers.

If you want to always reference the latest version, just reference the secretname without the guid. In this case if someone updates the secret in Azure Key-Vault your application would not notice.



    
    