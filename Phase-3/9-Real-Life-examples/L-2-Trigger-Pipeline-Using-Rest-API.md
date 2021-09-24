# Trigger Pipeline using Rest API
- Refrence: https://docs.microsoft.com/en-us/azure/data-factory/quickstart-create-data-factory-rest-api

## Steps
- Install Azure Power Shell using - https://github.com/Azure/azure-powershell/releases
- Install Azure CLI Modules - https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows?tabs=azure-cli
- Launch PowerShell
- Run the following command
```
$subscriptionId = "a35eb3a5-e30d-4dcb-9b4e-109d99e6628f"
Connect-AzAccount
Get-AzSubscription
Select-AzSubscription -SubscriptionId $subscriptionId
```

- Create Service Principal
```
az login
$appId = "adfsp1"
az ad sp create-for-rbac --name $appId
```


- Run the following commands after replacing the places-holders with your own values, to set global variables to be used in later steps.
```
$tenantID = "1e3e3dfb-4177-4e46-9ca5-29e178d50824"
$clientSecrets = "g-N4wYcC9z--QcNztVnIvMXpgr9IutU-JV"
$resourceGroupName = "rfsep21"
$factoryName = "dfatinsep21"
$apiVersion = "2018-06-01"
```

- Authenticate with Azure AD
```
$credentials = Get-Credential -UserName $appId
Connect-AzAccount -ServicePrincipal  -Credential $credentials -Tenant $tenantID
GetToken
```

- Create pipeline run with parameters
```
$path = "/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.DataFactory/factories/${factoryName}/pipelines/{pipeline-name}/createRun?api-version=2018"

$body = @"
{  
        "strParamInputFileName": "emp2.txt",
        "strParamOutputFileName": "aloha.txt"
}
"@

$response =  Invoke-AzRestMethod  -Path ${path}  -Method POST -Payload $body
$response.content
$runId  = ($response.content | ConvertFrom-Json).runId
```

- Monitor pipeline
```
$path = "/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.DataFactory/factories/${factoryName}/pipelineruns/${runId}?api-version=${apiVersion}"


    while ($True) {

        $response =  Invoke-AzRestMethod  -Path ${path}  -Method GET
        $response = $response.content | ConvertFrom-Json

        Write-Host  "Pipeline run status: " $response.Status -foregroundcolor "Yellow"

        if ( ($response.Status -eq "InProgress") -or ($response.Status -eq "Queued") -or ($response.Status -eq "In Progress") ) {
            Start-Sleep -Seconds 10
        }
        else {
            $response | ConvertTo-Json
            break
        }
    }
```

- Retrieve copy activity run details, for example, size of the data read/written.
```
$path = "/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.DataFactory/factories/${factoryName}/pipelineruns/${runId}/queryActivityruns?api-version=${apiVersion}"


    while ($True) {

        $response =  Invoke-AzRestMethod  -Path ${path}  -Method POST
        $responseContent = $response.content | ConvertFrom-Json
        $responseContentValue = $responseContent.value

        Write-Host  "Activity run status: " $responseContentValue.Status -foregroundcolor "Yellow"

        if ( ($responseContentValue.Status -eq "InProgress") -or ($responseContentValue.Status -eq "Queued") -or ($responseContentValue.Status -eq "In Progress") ) {
            Start-Sleep -Seconds 10
        }
        else {
            $responseContentValue | ConvertTo-Json
            break
        }
    }
```
