# endsem-cddjobs:
- deployment: DeploymentJob
pool:
  vmImage: $(vmImageName)
environment: $(environmentName)
strategy:
  runOnce:
    deploy:
      steps:

      - task: UsePythonVersion@0
        inputs:
          versionSpec: '$(pythonVersion)'
        displayName: 'Use Python version'

      - task: AzureWebApp@1
        displayName: 'Deploy Azure Web App : {{ webAppName }}'
        inputs:
          azureSubscription: $(azureServiceConnectionId)
          appName: $(webAppName)
          package: $(Pipeline.Workspace)/drop/$(Build.BuildId).zip

          # The following parameter is specific to the Flask example code. You may
          # or may not need a startup command for your app.

          startUpCommand: 'gunicorn --bind=0.0.0.0 --workers=4 startup:app'
