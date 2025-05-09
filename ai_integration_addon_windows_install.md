# Itiner Workspace AI Integration Addon Windows Installation Guide

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

> **Important:** If the AI Integration Addon and the Itiner Workspace application are installed on different servers, ensure that the firewall allows incoming connections on the port specified by `APP_PORT`. This is necessary for Workspace to successfully communicate with the addon.

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
WS_URL=...  # Workspace API endpoint for variable updates (adjust if not localhost)
WS_API_KEY=...  # Custom API key for authenticating with Workspace
WS_HEALTHCHECK_URL=...  # Health check URL used by Workspace to verify integration availability

# Security and routing
HMAC_SECRET=...  # Secret used for validating HMAC-signed requests from Workspace
X-CUSTOM-APIKEY=...  # Custom API key header (same as WS_API_KEY, used for secure communication)

# Event filtering
REFERENCE_FILTER=[]  # Optional list of references to filter incoming events (e.g., ["invoice", "contract"])

# Route and host customization
PATH_BASE=/api  # Base path prefix for all exposed endpoints
HOST_URL=http://localhost  # Publicly accessible host URL (used in response and callback payloads)
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
