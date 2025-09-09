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

## Tools Used

Here's the list of tools that I used for building the agentic expense tracker:
- N8N - A workflow automation platform which supports integrations with various apps and services. Also supports Python and JavaScript for custom code.
- Stirling PDF - A PDF manipulation tool. Used for unlocking password-protected PDF statements from banks.
- Paperless-ngx - Document management system which can parse PDF statements from images using Optical Character Recognition (OCR) or by text extraction.

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

