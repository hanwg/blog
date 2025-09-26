---
title: "Agentic Expense Tracking Workflow"
summary: "Tracking expenses with the help of AI and automation"
date: 2025-09-24
tags:
  - SoftwareEngineering
  - AI
cover:
   image: "posts/2025/09/agentic-expense-tracking-workflow/agentic-expense-tracking-workflow.png"
   alt: "Expense tracking"
   caption: "Expense Tracking"
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
In the following sections, I will be breaking down the agentic expense tracking workflow to explain how it works.

## The Import Transaction Workflow

The purpose of the `Import Transaction` workflow is to import transactions into my expense tracker, Firefly.  

![Import transaction workflow](../import-transaction-workflow.png)

Pre-conditions:
1. I perform a transaction (ATM withdrawal, credit card transaction or fund transfer).
2. The bank processes the transaction and sends an email notification containing the transaction details such as date, description and amount.

The flow:
1. The workflow begins when the `transaction created` email event is triggered.
2. Extract the text content of the email body from the email message.
3. Parse the email body using regular expressions to retrieve the transaction details.
4. Classify the transaction description using the AI agent to determine which expense category the transaction should belong to.
    - The agent is preconfigured with the necessary model and prompt to perform the classification.
    - Sample prompt:
      ```
      Classify the given transaction description using one of the categories:
      Food, Utilities, Transport, Healthcare, Others.
      Respond with only the category without any markup.
      The transaction description is: {{ $transactionDescription }}
      ```
5. Invoke the Firefly's `transactions` REST API to create the expense using the transaction details and expense category.
6. Label the transaction notification to avoid duplicate processing.
7. Optionally, send an email alert if a predefined monthly expenditure threshold is reached. (Great for tracking credit card usage!)

Side note: The above workflow is based on email events.
The workflow for push notification events is identical but uses webhooks instead to provide an endpoint for the trigger. 

## The Reconciliation Workflow

At the end of the month, I receive monthly statements of account which will be used for reconciliation.
This is also how I capture transactions (interests, forex, etc.) which do not have transaction notifications.

This workflow is split into 2 parts:
1. Archive PDF - Extracts PDF attachment from email and uploads it to Paperless-ngx.
2. Process PDF - Imports the transactions in the PDF into Firefly. 

### The Archive PDF Flow

The purpose of the `Archive PDF` workflow is to extract the text content from the PDF email attachment which contains the statement of account.

![Archive PDF workflow](../archive-pdf-workflow.png)

Pre-conditions: The bank sends email with an attachment containing the statement of account in a password-protected PDF.

The flow:
1. The workflow beings when the `statement of account` email event is triggered.
2. Extract the date from the email. This will be used for archiving purposes later.
3. Get the message from the email thread.
4. Get the attachment (password-protected PDF) from the message.
5. Save the PDF to disk by converting the attachment from Base64 String to binary file.
6. Invoke the Stirling PDF `remove-password` REST API with the binary file to unlock the PDF.
7. Label and tag (date along with other metadata such as bank name and account number) the unlocked PDF and upload it to Paperless-ngx for archival and processing.
    - Paperless-ngx extracts the text content from the PDF. This happens in the background within Paperless-ngx and is not part of the workflow.
8. Trigger the `Process PDF` workflow.

### The Process PDF flow

The `Process PDF` workflow is similar to the `Import Transaction` workflow above.
The `Import Transaction` workflow processes a single transaction from a transaction notification while the `Process PDF` workflow processes multiple transactions from the statement of account PDF.

![Archive PDF workflow](../process-pdf-workflow.png)

1. The workflow begins when triggered by the `Archive PDF` workflow.
2. Invoke the Paperless-ngx `documents` REST API to fetch the unlocked PDF.
3. Aggregate the text content from the PDF files.
4. Parse the transactions from the text content using regular expressions.
5. For each transaction,
    - Invoke the Firefly's `transactions` REST API to create the expense using the transaction details along with the expense category.
    - Invoke the Paperless-ngx `bulk_edit` REST API to remove tags from the PDF to avoid duplicate processing.
6. Format a summary message.
7. Send notification containing the summary.

## Challenges

1. **No standard format for transaction details.** Different banks use different formats, often requiring the use of various complex regular expressions for parsing. While I have a basic functioning solution, a more elegant approach would be the use of strategy pattern to identify which regular expression to use for any given transaction. 
2. **Credit card transactions have complicated "lifecycles".** Different types of credit card transactions may involve pre-authorizations, chargebacks, etc. Hence, the transaction doesn't end after importing it into the expense tracker; there might be further updates to the expense item and correlating the transactions with the expense may sometimes require manual intervention.

## Conclusion

We've seen how the agentic expense tracking workflow can free up valuable time and energy by automating the tedious and error-prone process of capturing and classifying transactions.
Beyond accuracy and efficiency, the agentic expense tracking workflow has also provided me with a clearer and up-to-date picture of my financial status.
This insight empowers me to make smarter decisions, a benefit that will only grow with the increasing adoption of cashless payments.
