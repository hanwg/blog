---
title: "Agentic Expense Tracking Workflow"
summary: "Tracking expenses with the help of AI and automation"
date: 2025-09-17
draft: true
tags:
  - SoftwareEngineering
  - AI
---
Expense tracking can feel like a chore - you input and categorize, and hope that you didn't miss anything or entered something wrongly.
Everyone has their own preferred way of tracking expenses, whether it's pen-and-paper, spreadsheets or expense tracking apps.
Personally, I opted for a custom low-code solution leveraging on AI and automation.

## The Setup

The solution involves integrating the following free open-sourced tools to create an agentic expense tracking workflow:

| Tool          | Remarks                                                                                                                                         |
|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| N8N           | A workflow automation platform which supports integrations with various apps and services. Also supports Python and JavaScript for custom code. |
| Stirling PDF  | A PDF manipulation tool. Used for unlocking password-protected PDF statements from banks.                                                       |
| Paperless-ngx | Document management system which can parse PDF statements from images using Optical Character Recognition (OCR) or by text extraction.          |
| Firefly       | Expense tracker that can import data through various sources such as CSV files or REST API.                                                     |

The key to the solution lies with the N8N workflows which orchestrate the automation and processing.
In the following sections, I will be introducing some of these workflows.

## The Import Transaction Workflow

The name says it all - the purpose of the `Import Transaction` workflow is to import transactions into my expense tracker, Firefly.  

![Import transaction workflow](../import-transaction-workflow.png)

Pre-conditions:
1. I perform a transaction (ATM withdrawal, credit card transaction or fund transfer).
2. The bank processes the transaction and sends an email notification containing the transaction details such as date, description and amount.

The flow:
1. The workflow begins when the `transaction created` email event is triggered.
2. The email body text content is extracted from the email message.
3. The email body is parsed using regular expressions to retrieve the transaction details.
4. The transaction description is sent to an agent to determine which expense category the transaction should belong to.
    - The agent is preconfigured with the necessary model and prompt to perform the classification.
    - Sample prompt:
      ```
      Classify the given transaction description using one of the categories:
      Food, Utilities, Transport, Healthcare, Others.
      Respond with only the category without any markup.
      The transaction description is: {{ $transactionDescription }}
      ```
5. The Firefly's `transactions` REST API is invoked to create the expense using the transaction details which also includes the expense category.
6. The transaction notification is labelled to avoid duplicate processing.
7. Optionally, an email alert is sent when a predefined monthly expenditure threshold is reached. (Great for tracking credit card usage!)

Side note: The above workflow is based on email events.
The workflow for push notification events is identical but uses webhooks instead to provide an endpoint for the trigger. 

## The Reconciliation Workflow

At the end of the month, I receive monthly statements of account which will be used for reconciliation.
This is also how I capture transactions (interests, forex, etc.) which do not have transaction notifications.

This workflow is split into 2 parts:
1. Archive PDF - Extracts PDF attachment from email and uploads it to Paperless-ngx.
2. Process PDF - Imports the transactions in the PDF into Firefly. 

### The Archive PDF Flow

![Archive PDF workflow](../archive-pdf-workflow.png)

Pre-conditions: The bank sends email with an attachment containing the statement of account in a password-protected PDF.

The flow:
1. The workflow beings when the `statement of account` email event is triggered.
2. The date is extracted from the email. This will be used for archiving purposes later.
3. Get the message from the email thread.
4. Get the attachment (password-protected PDF) from the message.
5. Save the PDF to disk by converting the attachment from Base64 String to binary file.
6. Invoke the Stirling PDF `remove-password` REST API with the binary file to unlock the PDF.
5. The unlocked PDF is uploaded to Paperless-ngx and labelled with the date (along with other metadata such as bank name and account number) for archival and processing.
    - Paperless-ngx extracts the text content from the PDF. This happens in the background within Paperless-ngx and is not part of the workflow.
6. The `Process PDF` workflow is triggered.

### The Process PDF flow

![Archive PDF workflow](../process-pdf-workflow.png)

1. The workflow begins when triggered by the `Archive PDF` workflow.
2. The workflowPaperless-ngx REST API
3. The text For each transaction,
      a. The transaction description is sent to an agent to determine which expense category the transaction should belong to.
      b. The workflow invokes Firefly's `transactions` REST API to create the expense using the transaction details along with the expense category.

## Key Challenges

1. **No standard format for transaction details**. Different banks use different formats, often requiring the use of various complex regular expressions for parsing.
2. **Credit card transactions are complicated**. Different types of credit card transactions have different workflows such as pre-authorizations, chargebacks, etc. Hence, the transaction "lifecycle" doesn't end after importing it into the expense tracker; there might be further updates to the expense item and correlating the transactions with the expense may sometimes require manual intervention.
