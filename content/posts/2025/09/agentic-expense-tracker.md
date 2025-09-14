---
title: "Agentic Expense Tracker"
summary: "Tracking expenses with the help of AI and automation"
date: 2025-09-01
draft: true
tags:
  - SoftwareEngineering
  - AI
---
Expense tracking can feel like a chore - you input and categorize, and hope that you didn't miss anything or entered something wrongly.
Everyone has their own preferred way of tracking expenses from pen-and-paper, spreadsheets to expense tracking apps.
Personally, I opted for a custom low-code solution leveraging on AI and automation.

Since none of my banks provide any form of integration with finance apps, I rely on transaction notifications (email) as my source data.
These transaction notifications would then be processed by my customized workflow powered by a low-code platform to import the transaction into my expense tracker.

## The Setup

Here's the list of tools that I used for building the agentic expense tracker:
- N8N - A workflow automation platform which supports integrations with various apps and services. Also supports Python and JavaScript for custom code.
- Stirling PDF - A PDF manipulation tool. Used for unlocking password-protected PDF statements from banks.
- Paperless-ngx - Document management system which can parse PDF statements from images using Optical Character Recognition (OCR) or by text extraction.
- Firefly - Expense tracker that can import data through various sources such as CSV files or REST API.

All of them are free, open-sourced and self-hosted.

## Import Transaction Workflow

The workflow is as follows:
1. I perform a transaction (ATM withdrawal, credit card transaction or fund transfer).
2. The bank processes the transaction and sends an email notification containing the transaction details such as date, amount and description.
3. The `transaction created` workflow event containing the email notification is triggered.
4. The email notification is parsed to retrieve the transaction details.
5. The transaction description is sent to an agent to determine which expense category the transaction should belong to.
    - The agent is preconfigured with the necessary model and prompt to perform the classification.
    - Sample prompt:
      ```
      Classify the given transaction description using one of the categories:
      Food, Utilities, Transport, Healthcare, Others.
      Respond with only the category without any markup.
      The transaction description is: {{ $transactionDescription }}
      ```
6. The workflow invokes the expense tracker's REST API to create the expense using the transaction details along with the expense category.

## Reconciliation Workflow

At the end of the month, I receive monthly statements of account which will be used for reconciliation.
This is also how I capture transactions involving interests and forex which do not have transaction notifications.

The reconciliation workflow is as follows:
1. The bank sends email with an attachment containing the statement of account in a password-protected PDF.
2. The password-protected PDF is unlocked using the Stirling PDF `remove-password` REST API. (Stirling PDF is a free open-sourced PDF manipulation tool)
3. The PDF is uploaded to Paperless-ngx for archival and processing.
4. Paperless-ngx extracts the text content from the PDF.
5. The text content is parsed to retrieve the transaction details.
6. The transaction description is sent to an agent to determine which expense category the transaction should belong to.
7. The workflow invokes the expense tracker's REST API to create the expense using the transaction details along with the expense category.
