# Infrastructure Setup Guide for Payslip Generator

This document outlines the setup, configuration, and interaction points for the core infrastructure components required for the AI-powered payslip generation application.

## 1. GPT-4 API (OpenAI)

*   **Objective:** To understand natural language user input, generate human-like explanations for applied payroll rules, and perform consistency checks as part of the triple verification process.
*   **Setup Steps:**
    1.  Create an OpenAI account at [https://platform.openai.com/](https://platform.openai.com/).
    2.  Navigate to the API keys section and generate a new secret key.
    3.  Securely store this API key.
    4.  Review OpenAI API documentation for usage policies, rate limits, and best practices.
    5.  Set up billing information as required for API usage.
*   **Configuration Placeholders:**
    *   `OPENAI_API_KEY`: Your unique API key from OpenAI.
    *   `OPENAI_MODEL_NAME`: (Optional, defaults to a standard GPT-4 model, but can be specified, e.g., "gpt-4-turbo-preview").
    *   *Storage:* These should be stored as environment variables on the server running the application/n8n or in a `.env` file loaded by the application.
*   **Integration & Interaction Points with the Application:**
    *   The main application backend (or n8n workflows) will make HTTPS POST requests to the OpenAI API endpoint (e.g., `https://api.openai.com/v1/chat/completions`).
    *   The `OPENAI_API_KEY` must be included in the `Authorization` header as a Bearer token (e.g., `Authorization: Bearer $OPENAI_API_KEY`).
    *   Prompts (as defined in `gpt_prompts_v1.md`) will be sent in the JSON request body, specifying the model and messages.
    *   The application will parse the JSON response from the API to extract the structured data, explanations, or validation results.

## 2. n8n (Self-Hosted or Cloud)

*   **Objective:** To act as the central orchestrator for the payslip generation workflow, connecting various services like WhatsApp, GPT-4, the Rule Engine, and the PDF Generator.
*   **Setup Steps (Self-Hosted Example using Docker):**
    1.  Ensure Docker and Docker Compose are installed on your server.
    2.  Create a `docker-compose.yml` file for n8n (refer to official n8n documentation for the latest recommended setup).
    3.  Configure necessary environment variables for n8n itself (e.g., domain name, user credentials, encryption key).
    4.  Run `docker-compose up -d` to start the n8n container.
    5.  Access the n8n UI via its configured URL and perform initial setup (admin user).
    *   *(For n8n Cloud, sign up at [https://n8n.cloud/](https://n8n.cloud/) and follow their onboarding.)*
*   **Configuration Placeholders (within n8n or its environment):**
    *   `N8N_WEBHOOK_URL_WATI`: The specific n8n webhook URL that Wati.io will call for incoming WhatsApp messages.
    *   `N8N_API_KEY`: (If external services need to trigger n8n workflows via its API).
    *   Credentials for other services (GPT-4, Wati, PDF API) will be configured within n8n's credential manager.
    *   *Storage:* n8n specific variables in its `docker-compose.yml` or server environment. Sensitive credentials for external services should be stored securely within n8n's built-in credential manager.
*   **Integration & Interaction Points with the Application:**
    *   n8n will be the central workflow engine.
    *   **Incoming:** It will receive incoming WhatsApp messages via a webhook provided by Wati.io.
    *   **Processing:**
        *   Workflows in n8n will make HTTP requests to the GPT-4 API for input parsing, justification, and validation.
        *   Workflows will interact with the Rule Engine. This could be via an HTTP request to a custom microservice/webhook endpoint where the rule engine logic resides, or potentially a custom n8n node if developed.
        *   Workflows will call the PDF Generator API with the finalized payslip data.
    *   **Outgoing:** n8n will use the Wati.io API to send messages (confirmations, questions, PDF payslips, text summaries) back to the user on WhatsApp.

## 3. WhatsApp API (via Wati.io)

*   **Objective:** To enable communication with users via WhatsApp for receiving payslip requests and sending back confirmations, clarifications, and the final payslip.
*   **Setup Steps (Wati.io Example):**
    1.  Register for a Wati.io account at [https://www.wati.io/](https://www.wati.io/).
    2.  Follow their process for obtaining WhatsApp Business API access (this may involve Facebook Business verification).
    3.  Set up your WhatsApp Business Profile.
    4.  Obtain API endpoint and access token from the Wati.io dashboard.
    5.  Configure a webhook in Wati.io to point to your n8n instance's webhook URL (e.g., `[N8N_BASE_URL]/webhook/[YOUR_WATI_WORKFLOW_ID]`).
*   **Configuration Placeholders:**
    *   `WATI_API_ENDPOINT`: The base URL for the Wati.io API (e.g., `https://api.wati.io/v1/`).
    *   `WATI_ACCESS_TOKEN`: Your Wati.io API token for authentication.
    *   `WATI_WHATSAPP_NUMBER`: The WhatsApp number associated with your Wati.io account.
    *   *Storage:* These should be stored as environment variables or, more securely, directly within n8n's credential manager.
*   **Integration & Interaction Points with the Application:**
    *   **Incoming Messages:** Wati.io will forward incoming messages from users on WhatsApp to a pre-configured n8n webhook URL. The payload will contain the message content and sender information.
    *   **Outgoing Messages:** n8n workflows (or the application backend via n8n) will make HTTPS POST requests to the Wati.io API (e.g., to `[WATI_API_ENDPOINT]/sendTemplateMessage` or `[WATI_API_ENDPOINT]/sendSessionMessage`) to send text messages, media (like PDFs), or interactive messages.
    *   The `WATI_ACCESS_TOKEN` will be included in the Authorization header for API requests to Wati.io.

*(Alternative: Twilio API for WhatsApp would follow a similar pattern: Sign up, get WhatsApp sender, configure webhook for incoming, use Twilio SDK/API for outgoing.)*

## 4. PDF Generator API (e.g., PDFMonkey, Api2Pdf)

*   **Objective:** To dynamically generate professional-looking PDF payslips based on structured data.
*   **Setup Steps (Generic Example):**
    1.  Choose a PDF Generator API provider (e.g., PDFMonkey, Api2Pdf, DocRaptor, Gotenberg if self-hosting a Dockerized one).
    2.  Sign up for an account with the chosen provider.
    3.  Obtain an API key from their dashboard.
    4.  Design a payslip template using their provided tools (often HTML/CSS based, or a visual editor). Note the template ID if applicable.
    5.  Review their API documentation for request formats and authentication.
*   **Configuration Placeholders:**
    *   `PDF_GENERATOR_API_KEY`: Your API key for the chosen PDF service.
    *   `PDF_GENERATOR_API_ENDPOINT`: The base URL for the PDF generator API.
    *   `PDF_GENERATOR_TEMPLATE_ID`: (If applicable) The ID of your pre-designed payslip template.
    *   *Storage:* Store as environment variables or within n8n's credential manager.
*   **Integration & Interaction Points with the Application:**
    *   After the Rule Engine and GPT-4 validation layers have finalized the payslip data, n8n (or the application backend) will prepare a JSON payload.
    *   This payload will contain all necessary information for the payslip: company details, employee information, earnings, deductions, net pay, legal mentions, rule justifications provided by GPT-4, etc.
    *   An HTTPS POST request will be made to the PDF Generator API endpoint (e.g., `[PDF_GENERATOR_API_ENDPOINT]/documents` or `[PDF_GENERATOR_API_ENDPOINT]/generate`).
    *   The `PDF_GENERATOR_API_KEY` will be used for authentication (typically in headers).
    *   The API will respond either with the PDF binary data directly, a URL to download the generated PDF, or a job ID to poll for completion.
    *   The application/n8n will then handle this response to either save the PDF or send it to the user via WhatsApp (through Wati.io).

---

**General Note on Security:**
All API keys and sensitive credentials must be stored securely using environment variables, secrets management tools (like HashiCorp Vault, AWS Secrets Manager, etc.), or the built-in credential managers of tools like n8n. Avoid hardcoding them directly into application code or committing them to version control. Use `.env` files for local development only, ensuring `.env` is in `.gitignore`.
