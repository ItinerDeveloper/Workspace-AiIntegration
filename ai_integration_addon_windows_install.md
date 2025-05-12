# Itiner Workspace Invoice Data Extraction Addon Installation Guide for Windows Server

This document outlines the installation and configuration process for the **Invoice Data Extraction Addon** for the Itiner Workspace platform, specifically for deployments on **Windows Server operating systems**.

---

## 1. Preparation

### 1.1 System Requirements
- Windows Server OS
- Administrative privileges for service installation
- Internet connection
- Installed Itiner Workspace application

### 1.2 File System
Unpack the Invoice Data Extraction Addon package to your preferred location, for example:
```
D:\ItinerWorkspace\InvoiceDataExtractionAddon\
```

### 1.3 Azure AI OCR Service Configuration

This addon uses **Azure Cognitive Services** for OCR (Optical Character Recognition), specifically configured for recognizing and extracting data from invoice documents.

#### Steps to Configure Azure Cognitive Services:

1. Log in to the [Azure Portal](https://portal.azure.com/).
2. Search for **Cognitive Services** and click **+ Create**.

**Basic Setup**:
- **Subscription**: Select your Azure subscription.
- **Resource Group**: Create a new one or reuse an existing group.
- **Region**: Select a region that supports **Form Recognizer** (e.g., *East US 2*, *West Europe*).
- **Name**: Assign a name (e.g., `invoice-ocr-service`).
- **Pricing Tier**: Use **Standard S0** or Free F0 tier if available.

3. Click **Review + Create** and then **Create**.

After deployment:
- Navigate to the resource overview page.
- Copy:
  - **Endpoint** → Use as `AZURE_COGNITIVE_ENDPOINT`
  - **Key1** or **Key2** → Use as `AZURE_API_KEY`

---

## 2. Configuration

### 2.1 Environment Configuration (`.env`)

Create or edit the `.env` file in the addon directory with the following structure:

```env
AZURE_COGNITIVE_ENDPOINT=...  # Azure OCR endpoint
AZURE_API_KEY=...  # Azure OCR API key

WS_URL=https://companydomain.com/workspace/api/integration/variables  # Workspace integration endpoint
WS_API_KEY=...  # API key for Workspace authentication
WS_HEALTHCHECK_URL=http://addonhostdnsname:8000/workspace/idrecognizer/healthcheck  # Health check URL

HMAC_SECRET=...  # HMAC secret for validating signed requests
X-CUSTOM-APIKEY=...  # Custom API header for authentication

REFERENCE_FILTER=[]  # Optional list of references to filter incoming events (e.g., ["invoice", "contract"]) 
PATH_BASE=/workspace/invoicedataextractor  # Base API path
HOST_URL=...  # Public host URL
VARIABLE_PREFIX=  # Prefix for embedded workflow variable names

#SEQ_SERVER_URL=http://localhost:5341/  # Optional SEQ log server
LOG_LEVEL=ERROR  # Logging level

SVC_NAME=WorkspaceInvoiceDataExtractionWindowsService  # Windows service name
SVC_DISPLAY_NAME=Workspace Invoice Data Extraction Service  # Service display name
SVC_DESCRIPTION=Workspace Invoice Data Extraction Service for Windows  # Service description

APP_HOST=...  # Hostname or IP address for the service
APP_PORT=...  # Listening port for the service
```

> **Important:** If the Invoice Data Extraction Addon and the Itiner Workspace application are installed on different servers, ensure that the firewall allows incoming connections on the port specified by `APP_PORT`. This is necessary for Workspace to successfully communicate with the addon.

---

## 3. Installation

### 3.1 Installing the Service

Open a Command Prompt as Administrator, navigate to the directory containing `wsinvoicedataextraction.exe`, and run:

```
wsinvoicedataextraction.exe install
```

This will register the addon as a Windows Service.

### 3.2 Starting the Service

To start the service, execute:

```
wsinvoicedataextraction.exe start
```

The service will begin listening on the configured host and port.

---

## 4. Logging

By default, logs are written to standard output. SEQ integration is optional and can be enabled by configuring the `SEQ_SERVER_URL` in the `.env` file. Adjust the `LOG_LEVEL` as needed.

---

## 5. Updating the Invoice Data Extraction Addon

1. **Stop** the service:
   ```
   wsinvoicedataextraction.exe stop
   ```
2. Replace or update files as needed.
3. **Restart** the service:
   ```
   wsinvoicedataextraction.exe start
   ```

---

This guide provides the installation and configuration steps for the Invoice Data Extraction Addon for Itiner Workspace. For further support, contact your system administrator or the Itiner Workspace development team.
