pipeline:
  agent:
    label: 'your-jenkins-agent'  # Replace with the label of your Jenkins agent with necessary tools (JDK, Maven)

  environment:
    AZURE_WEBAPP_NAME: 'heimdall-core'  # Replace with your Azure Web App name
    AZURE_RESOURCE_GROUP: 'MyResourceGroup'  # Replace with your Azure resource group name
    AZURE_WEBAPP_PUBLISH_PROFILE: 'your-azure-publish-profile'  # Replace with your Azure Web App publish profile credential ID in Jenkins

  stages:
    - stage: 'Build'
      steps:
        - script: 
            git 'https://github.com/your-heimdall-repo.git'  # Replace with the URL of your Heimdall project repository
        - script: 
            mvn clean package
      post:
        success:
          - archiveArtifacts:
              artifacts: 'target/*.jar'  # Adjust the path if your Maven build output path is different
          - cleanWs

    - stage: 'Deploy'
      steps:
        - script: 
            az webapp config set --name $AZURE_WEBAPP_NAME --resource-group $AZURE_RESOURCE_GROUP --java-version 11
            # If you need additional configurations, such as setting environment variables, you can add more 'az webapp config set' commands here

        - script: 
            az webapp deployment source config-zip --name $AZURE_WEBAPP_NAME --resource-group $AZURE_RESOURCE_GROUP --src 'target/*.jar'
            # This command deploys the packaged JAR file to your Azure Web App. Adjust the path if your Maven build output path is different

