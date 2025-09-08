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

## The Workflow

The workflow is as follows:
1. I perform a transaction (ATM withdrawal, credit card transaction or fund transfer).
2. The bank sends an email notification which contains the transaction details such as date, amount and description.
3. The `transaction created` workflow event is triggered.
4. The transaction notification is parsed to retrieve the transaction details.
5. The transaction description is sent to an agent to determine which expense category the transaction should belong to.
The agent is preconfigured with the necessary model and prompts to perform the classification.
6. The workflow invokes the REST API of the expense tracking app to create the expense using the transaction details along with the expense category.

## 
