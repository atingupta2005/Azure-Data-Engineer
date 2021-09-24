# Executing-Pipeline-from-Azure-Logic-Apps

- Create Logic App
- Create ADF
- Create Managed Identity in Logic App
- Provide role - "Contributor" in ADF to Logic App
- Create a sample pipeline in ADF
- Publish the pipeline
- Open Logic App and create a HTTP trigger workflow
- URI - https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.DataFactory/factories/${factoryName}/pipelines/{pipeline-name}/createRun?api-version=2018-06-01
- Change the variables in URL
- Add new parameter for Authentication
- Authentication Type - Managed Identity -> System Assigned
- Run Logic App
