[[_TOC_]]

# CI
## Pipeline
  - Name:```FastAPI-CI```
  - Agent pool:```Azure Pipelines```
  - Agent Specification:```ubuntu-latest```
![image.png](/.attachments/image-4a2d4782-910c-45f6-9bbf-8b0fb28db13d.png)
## Agent job
  - Display name:```Build and Test```
![image.png](/.attachments/image-38944ef9-8f98-4dc1-b15a-9e619a703491.png)
## Use Python version
  - Display name:```Use Python $(python.version)```
  - Version spec:```$(python.version)```
![image.png](/.attachments/image-822f1e1a-0600-4374-a55b-db6f9c198742.png)
## Command line
  - Display name:```Install dependencies```
  - Script:<br>```python -m venv antenv```
```source antenv/bin/activate```
```python -m pip install --upgrade pip```
```pip install setup```
```pip install --target="./.python_packages/lib/site-packages" -r ./requirements.txt```
![image.png](/.attachments/image-f6ae3903-f083-4377-9f92-8c1785e8ad0a.png)
## Archive files
  - Display name:```Archive $(System.DefaultWorkingDirectory)```
  - Root folder or file to archive:```$(System.DefaultWorkingDirectory)```
  - Archive type:```zip```
  - Archive file to create:```$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip```
![image.png](/.attachments/image-a0e6072e-8b18-45eb-89b2-a022892af988.png)
## Publish build artifacts
  - Display name:```Publish Artifact: drop```
  - Path to publish:```$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip```
  - Artifact name:```drop```
  - Artifact publish location:```Azure Pipelines```
![image.png](/.attachments/image-950d6f07-1e4d-41c9-b61b-ca0d5986ae87.png)

# CD
## Artifact
  - Project:```SandBox```
  - Source (build pipeline):```SandBox-FastAPI-Test-CI```
  - Default version:```Latest```
  - Source alias:```_SandBox-FastApiTest-CI```
![image.png](/.attachments/image-64a696fa-1224-4242-8b85-5977ffd9e6c2.png)
## Stage
  - Stage name:```Stage 1```
  - Azure subscription:```WALSIN-Azure-Sandbox```
  - App type:```Web App on Linux```
  - App service name:```app-stone-dev```
  - Startup command:```gunicorn -w 2 -k uvicorn.workers.UvicornWorker -b 0.0.0.0:8000 App.main:app```
![image.png](/.attachments/image-fd56a898-971a-4882-9c42-c7dc78d9100a.png)
## Agent job
  - Display name:```Run on agent```
  - Agent Specification:```ubuntu latest```
![image.png](/.attachments/image-55bdfe97-b4fc-4573-986c-44c80e276e38.png)
## Azure App Service deploy
  - Display name:```Deploy Azure App Service```
  - Connection type:```WALSIN-Azure-Sandbox```
  - App Service type:```Web App on Linux```
  - App Service name:```app-stone-dev```
  - Package or folder:```$(System.DefaultWorkingDirectory)/**/*.zip```
![image.png](/.attachments/image-bbdec3a1-bd9c-4a82-bc01-12960c5a225f.png)

