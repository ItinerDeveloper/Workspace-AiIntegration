# AI Integration Service

## Service Route
`/aiintegration/aiintegration`

## Description
The AI Integration Service allows users to call the MS Azure OpenAI Service and leverage the GPT o1-mini model from any Itiner Workspace workflow using a prompt specified in the `aiPrompt` variable.  
It is triggered via a webhook set up within a workflow. The service then writes the response generated by the AI back into the workflow's `aiAnswer` variable. Users can also attach documents to the prompt, which the AI can summarize or analyze according to the prompt's requirements.

> 💡 **Synchronous Operation**: This integration service operates synchronously. It receives the export event from the workflow and returns the result, including updates to variables such as `aiAnswer`, within the same session. This ensures immediate availability of AI-generated data in the ongoing workflow process.

### Feature: Document Analysis  
Users can send a document along with a prompt to the AI model to extract insights, generate summaries, or analyze its content. This feature is useful for tasks such as summarizing contract terms, identifying key points in reports, or extracting relevant details from legal documents.

#### Supported File Types for OCR:
- `.png`
- `.jpeg`
- `.jpg`
- `.bmp`
- `.tiff`
- `.heif`

> **Note:** PDF files are supported as well, **but** to indicate that a PDF should be processed via OCR, its filename must include an `ocr` substring.  
> Example: `invoice_ocr.pdf`

For example, a user can provide the following prompt:  
> "Summarize the key obligations and responsibilities outlined in the attached contract."

The AI model processes the document and returns a structured response based on the request, which is then stored in the `aiAnswer` variable within the workflow.

### Feature: Variable Rule Extraction
The AI Integration Service now supports extracting structured data from documents based on user-defined variable rules. Users can specify in their prompt how extracted information should be structured in natural language, and the AI model will generate a JSON object accordingly.  The Integration Service will then take the JSON object, and map each field to its Itiner Workspace template variable counterpart by matching name and type. The JSON object will also be sent back to Itiner Workspace and injected into the `aiAnswer` variable of the relevant workflow.

For example, a user can include the following instruction in their prompt:
> "Prepare a JSON object that will contain data extracted from the given document, with the following structure:  
> - `aiInvoiceId` (string) should contain the identification number of the invoice.  
> - `aiInvoiceAmount` (number) should contain the invoice amount.  
> - `aiDueDate` (date) should contain the due date of the invoice."

If the extracted data matches defined workflow variables in both name and type, the integration service automatically maps and imports them into the workflow.

## Dedicated Template Variables
- **aiPrompt**: The variable containing the prompt to be sent to the MS Azure endpoint.
- **aiAnswer**: The variable where the response from the AI will be stored.

---

This service is part of the Itiner Workspace integrations and is designed to facilitate AI-powered interactions within workflows by leveraging MS Azure OpenAI Service capabilities.

For more information regarding the MS Azure OpenAI Service, please see the [MS Azure OpenAI Service documentation](https://learn.microsoft.com/en-us/azure/ai-services/openai/).
