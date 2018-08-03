
### Login

```
az cloud set -n AzureChinaCloud
```

```
az login

az account set --subscription "24da320e-0d3c-41e2-b3c4-6e3b73ce8281"

### Create Key Valut

```
az group create -n rgkeyvalut01 -l "chinaeast"
az keyvault create -n kv01 -g rgkeyvalut01 -l "chinaeast" --enabled-for-template-deployment true
az keyvault secret set --vault-name kv01 -n ocsecret01 --file "C:\Users\markp\OneDrive\Essential Tools\Putty\acs.openssh"
```

### Create AAD
az ad sp create-for-rbac -n openshiftcloudprovider --password Password01! --role contributor --scopes /subscriptions/24da320e-0d3c-41e2-b3c4-6e3b73ce8281
```javascript
{
  "appId": "58d35f9c-992d-4bb5-9d0a-a109f1c79af7",
  "displayName": "openshiftcloudprovider",
  "name": "http://openshiftcloudprovider",
  "password": "Password01!",
  "tenant": "4b457c1d-8d51-4682-893a-d415dc78616c"
}
```

### Deploy
!! should modify imageReference to
```
                    "imageReference": {
                        "publisher": "yungoalbj",
                        "offer": "redhat",
                        "sku": "redhat_en-us_7_3_0",
                        "version": "latest"
                    },
```

Modify _artifactsLocation to your own github url

Should modify CloudName from "AzurePublicCloud" to "AzureChinaCloud" in the deployOpenShift.sh file
     if [[ $CLOUD == "US" ]]
       then
         DOCKERREGISTRYYAML=dockerregistrygov.yaml
         export CLOUDNAME="AzureUSGovernmentCloud"
     else
         DOCKERREGISTRYYAML=dockerregistrypublic.yaml
         export CLOUDNAME="AzureChinaCloud"
     fi

az group create -n rgmarkocp -l "chinaeast"
cd C:\Users\markp\Documents\github\openshift-container-platform
az group deployment create --name markocpdeployment --template-file azuredeploy.json --parameters azurecndeploy.parameters.json --resource-group rgmarkocp --no-wait



error:
Message: Deployment template validation failed: 'The template parameters 'addressPrefix' in the parameters file are not valid; they are not present in the original template and can therefore not be provided at deployment time. The only supported parameters for this template are '_artifactsLocation, masterVmSize, infraVmSize, nodeVmSize, cnsVmSize, storageKind, openshiftClusterPrefix, masterInstanceCount, infraInstanceCount, nodeInstanceCount, dataDiskSize, adminUsername, openshiftPassword, enableMetrics, enableLogging, enableCNS, rhsmUsernameOrOrgId, rhsmPasswordOrActivationKey, rhsmPoolId, sshPublicKey, keyVaultResourceGroup, keyVaultName, keyVaultSecret, enableAzure, aadClientId, aadClientSecret, defaultSubDomainType, defaultSubDomain, virtualNetworkNewOrExisting, virtualNetworkResourceGroupName, virtualNetworkName, addressPrefixes, masterSubnetName, masterSubnetPrefix, nodeSubnetName, nodeSubnetPrefix'. Please see https://aka.ms/arm-deploy/#parameter-file for usage details.'.


Message: Deployment template validation failed: 'The provided value 'Standard_D4_v2' for the template parameter 'cnsVmSize' at line '1' and column '4634' is not valid. The parameter value is not part of the allowed value(s): 'Standard_D4s_v3,Standard_D8s_v3,Standard_D16s_v3,Standard_D32s_v3,Standard_D64s_v3,Standard_E4s_v3,Standard_E8s_v3,Standard_E16s_v3,Standard_E32s_v3,Standard_E64s_v3'.'.