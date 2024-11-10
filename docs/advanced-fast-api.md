# VWC FastAPI Advanced Operations Guide 🎖️

## Part 1: Advanced FastAPI Features

### WebSocket Implementation (Real-time Communications)

```python
from fastapi import FastAPI, WebSocket, WebSocketDisconnect
from typing import List

app = FastAPI()

class CommandCenter:
    def __init__(self):
        self.active_connections: List[WebSocket] = []

    async def connect(self, websocket: WebSocket):
        await websocket.accept()
        self.active_connections.append(websocket)

    def disconnect(self, websocket: WebSocket):
        self.active_connections.remove(websocket)

    async def broadcast(self, message: str):
        for connection in self.active_connections:
            await connection.send_text(message)

command_center = CommandCenter()

@app.websocket("/ws/mission-updates/{unit_id}")
async def mission_updates(websocket: WebSocket, unit_id: str):
    await command_center.connect(websocket)
    try:
        while True:
            data = await websocket.receive_text()
            await command_center.broadcast(f"Unit {unit_id}: {data}")
    except WebSocketDisconnect:
        command_center.disconnect(websocket)
        await command_center.broadcast(f"Unit {unit_id} disconnected")
```

### File Operations with Azure Storage

```python
from fastapi import UploadFile, File
from azure.storage.blob import BlobServiceClient
import os

class AzureStorageManager:
    def __init__(self):
        conn_str = os.getenv("AZURE_STORAGE_CONNECTION_STRING")
        self.blob_service_client = BlobServiceClient.from_connection_string(conn_str)
        self.container_name = "mission-docs"

    async def upload_file(self, file: UploadFile):
        blob_client = self.blob_service_client.get_blob_client(
            container=self.container_name,
            blob=file.filename
        )
        data = await file.read()
        await blob_client.upload_blob(data)
        return blob_client.url

    async def download_file(self, filename: str):
        blob_client = self.blob_service_client.get_blob_client(
            container=self.container_name,
            blob=filename
        )
        return await blob_client.download_blob()

storage_manager = AzureStorageManager()

@app.post("/upload-document/")
async def upload_document(file: UploadFile = File(...)):
    try:
        url = await storage_manager.upload_file(file)
        return {"status": "success", "url": url}
    except Exception as e:
        return {"status": "error", "message": str(e)}
```

### Rate Limiting

```python
from fastapi import FastAPI, Request
from fastapi.middleware.cors import CORSMiddleware
from slowapi import Limiter, _rate_limit_exceeded_handler
from slowapi.util import get_remote_address

limiter = Limiter(key_func=get_remote_address)
app = FastAPI()
app.state.limiter = limiter
app.add_exception_handler(429, _rate_limit_exceeded_handler)

@app.get("/secure-intel")
@limiter.limit("5/minute")
async def get_secure_intel(request: Request):
    return {"intel": "Classified information"}
```

### Caching with Azure Redis Cache

```python
from fastapi_cache import FastAPICache
from fastapi_cache.backends.redis import RedisBackend
from fastapi_cache.decorator import cache
import aioredis

@app.on_event("startup")
async def startup():
    redis = aioredis.from_url(
        os.getenv("AZURE_REDIS_CONNECTION_STRING"),
        encoding="utf8"
    )
    FastAPICache.init(RedisBackend(redis), prefix="vwc-cache")

@app.get("/mission-brief/{mission_id}")
@cache(expire=60)
async def get_mission_brief(mission_id: str):
    return {"mission_id": mission_id, "brief": "Mission details..."}
```

## Part 2: Azure Deployment Operations

### Azure App Service Configuration

```python
# config.py
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    APP_NAME: str = "VWC API"
    AZURE_STORAGE_CONNECTION_STRING: str
    AZURE_REDIS_CONNECTION_STRING: str
    AZURE_SQL_CONNECTION_STRING: str
    APPLICATIONINSIGHTS_CONNECTION_STRING: str

    class Config:
        env_file = ".env"
```

### Container Registry Setup

```dockerfile
# Dockerfile
FROM python:3.9-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY ./app ./app

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

### Azure DevOps Pipeline

```yaml
# azure-pipelines.yml
trigger:
- main

variables:
  azureSubscription: 'VWC-Azure'
  containerRegistry: 'vwccontainers.azurecr.io'
  imageRepository: 'vwc-api'
  dockerfilePath: 'Dockerfile'
  tag: '$(Build.BuildId)'

stages:
- stage: Build
  jobs:
  - job: BuildAndTest
    pool:
      vmImage: 'ubuntu-latest'
    steps:
    - task: Docker@2
      inputs:
        containerRegistry: '$(containerRegistry)'
        repository: '$(imageRepository)'
        command: 'buildAndPush'
        Dockerfile: '$(dockerfilePath)'
        tags: '$(tag)'

- stage: Deploy
  jobs:
  - job: Deploy
    pool:
      vmImage: 'ubuntu-latest'
    steps:
    - task: AzureWebAppContainer@1
      inputs:
        azureSubscription: '$(azureSubscription)'
        appName: 'vwc-api'
        containers: '$(containerRegistry)/$(imageRepository):$(tag)'
```

### Azure Monitor Integration

```python
# monitoring.py
from opencensus.ext.azure.trace_exporter import AzureExporter
from opencensus.trace.tracer import Tracer
from opencensus.trace.samplers import ProbabilitySampler
import logging

class AzureMonitoring:
    def __init__(self, connection_string: str):
        self.tracer = Tracer(
            exporter=AzureExporter(connection_string=connection_string),
            sampler=ProbabilitySampler(1.0)
        )
        
    def track_request(self, name: str):
        with self.tracer.span(name=name) as span:
            span.add_attribute("service", "vwc-api")
            return span

# Usage in main.py
monitoring = AzureMonitoring(os.getenv("APPLICATIONINSIGHTS_CONNECTION_STRING"))

@app.middleware("http")
async def monitoring_middleware(request: Request, call_next):
    with monitoring.track_request(f"{request.method} {request.url.path}"):
        response = await call_next(request)
        return response
```

### Azure Key Vault Integration

```python
# security.py
from azure.keyvault.secrets import SecretClient
from azure.identity import DefaultAzureCredential

class SecureConfig:
    def __init__(self):
        credential = DefaultAzureCredential()
        vault_url = os.getenv("AZURE_KEYVAULT_URL")
        self.client = SecretClient(vault_url=vault_url, credential=credential)

    def get_secret(self, name: str) -> str:
        return self.client.get_secret(name).value

# Usage
secure_config = SecureConfig()
db_password = secure_config.get_secret("database-password")
```

### Production Configuration

```python
# production.py
from azure.identity import DefaultAzureCredential
from fastapi.middleware.cors import CORSMiddleware

def configure_production(app: FastAPI):
    # CORS configuration
    app.add_middleware(
        CORSMiddleware,
        allow_origins=["https://*.azurewebsites.net"],
        allow_credentials=True,
        allow_methods=["*"],
        allow_headers=["*"],
    )

    # Azure authentication
    credential = DefaultAzureCredential()

    # Health check endpoint
    @app.get("/health")
    async def health_check():
        return {"status": "healthy", "environment": "azure-production"}
```

## Field Exercise: Complete Azure-Deployed API

Here's a complete example combining these concepts:

```python
# main.py
from fastapi import FastAPI, Depends, HTTPException, Request
from azure.identity import DefaultAzureCredential
from azure.storage.blob import BlobServiceClient
from azure.keyvault.secrets import SecretClient
import logging

class AzureAPI:
    def __init__(self):
        self.app = FastAPI(title="VWC Mission Control")
        self.credential = DefaultAzureCredential()
        self.setup_azure_services()
        self.setup_routes()
        self.setup_monitoring()

    def setup_azure_services(self):
        # Key Vault setup
        vault_url = os.getenv("AZURE_KEYVAULT_URL")
        self.key_vault = SecretClient(
            vault_url=vault_url,
            credential=self.credential
        )

        # Blob storage setup
        storage_connection = self.key_vault.get_secret("storage-connection").value
        self.blob_service = BlobServiceClient.from_connection_string(storage_connection)

    def setup_monitoring(self):
        connection_string = self.key_vault.get_secret("appinsights-connection").value
        self.monitoring = AzureMonitoring(connection_string)

    def setup_routes(self):
        @self.app.get("/")
        async def root():
            return {"status": "operational", "service": "VWC API"}

        @self.app.post("/documents/")
        async def upload_document(file: UploadFile = File(...)):
            try:
                container_client = self.blob_service.get_container_client("documents")
                blob_client = container_client.get_blob_client(file.filename)
                data = await file.read()
                await blob_client.upload_blob(data)
                return {"status": "success", "url": blob_client.url}
            except Exception as e:
                logging.error(f"Upload failed: {str(e)}")
                raise HTTPException(status_code=500, detail=str(e))

# Initialize application
azure_api = AzureAPI()
app = azure_api.app
```

## Deployment Commands

```bash
# Login to Azure
az login

# Create resource group
az group create --name vwc-resources --location eastus

# Create App Service plan
az appservice plan create \
    --name vwc-service-plan \
    --resource-group vwc-resources \
    --sku B1 \
    --is-linux

# Create Web App
az webapp create \
    --resource-group vwc-resources \
    --plan vwc-service-plan \
    --name vwc-fastapi-app \
    --runtime "PYTHON:3.9"

# Configure deployment
az webapp config appsettings set \
    --resource-group vwc-resources \
    --name vwc-fastapi-app \
    --settings \
        WEBSITES_PORT=8000 \
        SCM_DO_BUILD_DURING_DEPLOYMENT=true
```

## Next Mission Parameters

1. Advanced Azure Features:
   - Azure Functions integration
   - Azure API Management
   - Azure Front Door setup
   - Virtual Networks configuration

2. Security Hardening:
   - Managed Identities
   - Role-Based Access Control
   - Network Security Groups
   - Web Application Firewall

Remember:

- Monitor your resources
- Set up proper logging
- Configure automated backups
- Use managed identities
- Follow least privilege principle

🇺🇸 #VetsWhoCode
