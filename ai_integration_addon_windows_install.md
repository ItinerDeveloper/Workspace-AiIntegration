# Itiner Workspace AI Integration Addon Installation Guide for Windows Server

This document outlines the installation and configuration process for the **AI Integration Addon** for the Itiner Workspace platform, specifically for deployments on **Windows Server operating systems**.

---

## 1. Preparation

### 1.1 System Requirements
- Windows Server OS
- Administrative privileges for service installation
- Internet connection
- Installed Itiner Workspace application

### 1.2 File System
Unpack the AI Integration Addon package to your preferred location, for example:
```
D:\ItinerWorkspace\AIIntegrationAddon\
```

### 1.3 Azure AI Service Configuration

The AI Integration Addon requires two Azure AI resources:
- **Azure OpenAI** – for Language Model (LLM) operations like prompt completion.
- **Azure Cognitive Services** – for OCR operations via Form Recognizer.

#### 1.3.1 Creating an Azure OpenAI Resource (LLM)

1. **Log in** to the [Azure Portal](https://portal.azure.com/).
2. In the top search bar, type and select **Azure AI Services**.
3. Click **+ Create**.

**Basic Setup**:
- **Subscription**: Choose your Azure subscription.
- **Resource group**: Create a new one or select an existing one.
- **Region**: Select a supported region (e.g., *East US 2*, *West Europe*).
- **Name**: Provide a unique name (e.g., `workspace-openai`).
- **Pricing Tier**: Choose based on usage; standard is sufficient for most.

4. Click **Next** through the tabs, review the details, and click **Create**.

After deployment:
- Go to the resource overview page.
- Copy:
  - **Endpoint** → Use as `AZURE_OPENAI_ENDPOINT`
  - **Key1** or **Key2** from "Keys and Endpoint" → Use as `AZURE_OPENAI_API_KEY`

#### 1.3.2 Creating a Cognitive Services Resource (OCR)

1. In the Azure Portal, search for **Azure AI Services**.
2. Click **+ Create**.

**Basic Setup**:
- **Subscription**: Select your subscription.
- **Resource group**: Use the same one as the OpenAI service if desired.
- **Region**: Must support **Form Recognizer** (e.g., *East US 2*, *West Europe*).
- **Name**: Choose a unique name (e.g., `workspace-ocr`).
- **Pricing Tier**: Choose **Standard S0** or the free tier for testing.

3. Click **Next** to review and **Create** the resource.

After deployment:
- Go to the resource overview.
- Copy:
  - **Endpoint** → Use as `AZURE_COGNITIVE_ENDPOINT`
  - **Key1** or **Key2** → Use as `AZURE_API_KEY`

---

## 2. Configuration

### 2.1 Environment Configuration (`.env`)

Create or edit the `.env` file in the addon directory with the following structure:

```env
# Azure OpenAI configuration (LLM)
AZURE_OPENAI_API_KEY=...  # Azure API key for OpenAI (LLM) service
AZURE_OPENAI_ENDPOINT=...  # Endpoint of the Azure OpenAI (LLM) resource

# Azure Cognitive Services configuration (OCR)
AZURE_COGNITIVE_ENDPOINT=...  # Endpoint for Azure AI OCR (Form Recognizer)
AZURE_API_KEY=...  # API key for Azure AI OCR Services

# Workspace integration configuration
WS_URL=https://companydomain.com/workspace/api/integration/variables  # Workspace integration endpoint
WS_API_KEY=...  # API key for Workspace authentication
WS_HEALTHCHECK_URL=http://addonhostdnsname:8000/workspace/idrecognizer/healthcheck  # Health check URL

# Security and routing
HMAC_SECRET=...  # Secret used for validating HMAC-signed requests from Workspace
X-CUSTOM-APIKEY=...  # Custom API key header (same as WS_API_KEY, used for secure communication)

# Event filtering
REFERENCE_FILTER=[]  # Optional list of references to filter incoming events (e.g., ["invoice", "contract"])

# Route and host customization
PATH_BASE=/workspace/aiintegration  # Base path prefix for all exposed endpoints
HOST_URL=...  # Publicly accessible host URL (used in response and callback payloads)
VARIABLE_PREFIX=  # Used if this integration is triggered by an embedded workflow with prefixed variable names

# Optional logging configuration
#SEQ_SERVER_URL=http://localhost:5341/  # Optional SEQ logging server URL (uncomment to enable)
LOG_LEVEL=DEBUG  # Logging verbosity (DEBUG, INFO, WARNING, ERROR)

# Windows service configuration
SVC_NAME=WorkspaceAIIntegrationWindowsService  # Internal Windows service name
SVC_DISPLAY_NAME=Workspace AI Integration Service  # Display name in the Services list
SVC_DESCRIPTION=Workspace AI Integration Service for Windows  # Description of the Windows service

# Application host settings
APP_HOST=...  # DNS name of the server hosting the addon
APP_PORT=...  # Port number for the integration service (ensure it's available)
```

> **Important:** If the AI Integration Addon and the Itiner Workspace application are installed on different servers, ensure that the firewall allows incoming connections on the port specified by `APP_PORT`. This is necessary for Workspace to successfully communicate with the addon.

Ensure all required values are correctly configured before proceeding to installation.

---

## 3. Installation

### 3.1 Installing the Service

Open a Command Prompt as Administrator, navigate to the folder containing `wsaiintegration.exe`, and run:

```
wsaiintegration.exe install
```

This command registers the addon as a Windows Service using the service name and display parameters defined in the `.env` file.

### 3.2 Starting the Service

After successful installation, start the service with:

```
wsaiintegration.exe start
```

The AI Integration Addon will now run in the background as a Windows Service, listening on the configured host and port.

---

## 4. Logging

By default, logs are written to a file or optionally to SEQ if configured. Logging level can be adjusted with the `LOG_LEVEL` parameter in the `.env` file.

---

## 5. Updating the AI Integration Addon

1. **Stop** the service:
   ```
   wsaiintegration.exe stop
   ```
2. Replace the executable or configuration files as needed.
3. **Restart** the service:
   ```
   wsaiintegration.exe start
   ```

---

This guide provides the installation and configuration steps for the AI Integration Addon for Itiner Workspace. For further support, contact your system administrator or the Itiner Workspace development team.

