# n8n AI Sales Data Analyst Chatbot

This n8n workflow template allows you to build an AI-powered chatbot capable of connecting to your datasets (Google Sheets, PostgreSQL, MySQL), retrieving information, and performing calculations to answer your data-related questions in a conversational manner.

It's designed to be an easy and efficient starting point for AI-driven data analysis, making complex data queries accessible through natural language.

## Features

*   **AI-Powered Natural Language Queries:** Interact with your data using plain English (or other languages supported by the LLM).
*   **Multiple Data Source Integration:**
    *   Google Sheets
    *   PostgreSQL Databases
    *   MySQL Databases
*   **On-the-Fly Calculations:** Perform mathematical operations on retrieved data or user-provided numbers.
*   **Extensible:** Easily replace or add more data source tools (e.g., other databases, APIs).
*   **Chat-Based Interaction:** Uses the n8n chat interface for easy communication.
*   **Streamlined Data Analysis:** Automates repetitive data lookup and basic calculation tasks.

## How It Works

The core of this workflow is the **AI Agent** node. Here's a breakdown of the flow:

1.  **Chat Trigger:** You send a message (your question or command) via the n8n chat interface.
2.  **AI Agent:**
    *   Receives your message.
    *   Uses a Large Language Model (LLM) like OpenAI's GPT.
    *   Its **System Prompt** instructs it on its role (Sales Data Analyst), the tools available, and how to use them.
    *   Based on your query, the AI Agent decides which tool is needed (e.g., fetch data from Google Sheets, query a database, or perform a calculation).
    *   It then formulates the necessary input for the selected tool (e.g., Spreadsheet ID and range for Google Sheets, an SQL query for a database, or a mathematical expression for the calculator).
3.  **Tool Execution:**
    *   **Google Sheets Node:** If the AI decides to get data from Google Sheets, this node executes the request, fetching data from the specified `spreadsheetId` and `range`.
    *   **PostgreSQL/MySQL Node:** If a database query is needed, the respective node executes the SQL `query` provided by the AI.
    *   **Math Node (Calculator):** If a calculation is needed, this node evaluates the `expression` provided by the AI.
4.  **Response Generation:**
    *   The output from the executed tool is sent back to the AI Agent.
    *   The AI Agent processes this information and formulates a natural language response to your original query.
5.  **Chat Output:** The AI's response is displayed back to you in the n8n chat interface.

## Who is this template for?

*   **Data Analysts & Researchers:** Quickly pull data from various sources and perform calculations without writing complex scripts for every query.
*   **Developers & AI Enthusiasts:** A great starting point to learn how to build AI Agents with tool-using capabilities in n8n.
*   **Business Owners:** Get quick insights from your sales data without needing deep technical expertise.
*   **Automation Experts:** Enhance existing automation workflows by integrating AI-driven data analysis.

## Prerequisites

Before you can use this workflow, you'll need:

1.  **An n8n instance:** Either self-hosted or an n8n cloud account.
2.  **LLM API Access:**
    *   An **OpenAI API key** (or credentials for another supported LLM provider configured in n8n).
3.  **Data Source Credentials (configured in n8n):**
    *   **Google Sheets:** Google OAuth2 credentials.
    *   **PostgreSQL:** Connection credentials for your PostgreSQL database.
    *   **MySQL:** Connection credentials for your MySQL database.
4.  **Sample Data:**
    *   A Google Sheet with some sample sales data. You'll need its **Spreadsheet ID** and the **data range** (e.g., `Sheet1!A1:D100`).
    *   (Optional) PostgreSQL/MySQL databases with relevant sales tables.

## Setup Instructions

1.  **Import the Workflow:**
    *   Copy the JSON code provided for the workflow.
    *   In your n8n canvas, right-click and select "Paste" or use the "Import from Clipboard" option.

2.  **Configure Credentials (CRITICAL):**
    This is the most important step. The workflow will not function without correctly configured credentials.

    *   **AI Agent Node (`AI Sales Analyst Agent`):**
        *   Select this node.
        *   In the "Credential for OpenAI (Chat Models)" (or your LLM provider) field, select your configured OpenAI API credential.
        *   *(The JSON uses `YOUR_OPENAI_CREDENTIAL_ID` as a placeholder; ensure this is replaced by your actual credential selection).*

    *   **Google Sheets Node (`Get Google Sheet Data Tool`):**
        *   Select this node.
        *   In the "Credential for Google Sheets" field, select your configured Google Sheets OAuth2 credential.
        *   *(The JSON uses `YOUR_GOOGLE_SHEETS_CREDENTIAL_ID`; ensure this is replaced).*

    *   **PostgreSQL Node (`Query Postgres DB Tool`):**
        *   Select this node.
        *   In the "Credential for PostgreSQL" field, select your configured PostgreSQL credential.
        *   *(The JSON uses `YOUR_POSTGRES_CREDENTIAL_ID`; ensure this is replaced).*

    *   **MySQL Node (`Query MySQL DB Tool`):**
        *   Select this node.
        *   In the "Credential for MySQL" field, select your configured MySQL credential.
        *   *(The JSON uses `YOUR_MYSQL_CREDENTIAL_ID`; ensure this is replaced).*

3.  **Review System Prompt (Optional but Recommended):**
    *   Open the `AI Sales Analyst Agent` node.
    *   Review the `System Message`. This message defines the AI's behavior, its knowledge of the tools, and how it should interact. You can customize this prompt to better suit your specific needs or data structure.

4.  **Save the Workflow:**
    *   Click the "Save" button in the n8n editor.

5.  **Activate the Workflow:**
    *   Toggle the "Active" switch to "On" in the top right of the n8n editor.

## How to Use

Once the workflow is set up and active:

1.  Open the **Chat interface** in n8n (usually a chat bubble icon in the bottom-right corner of the editor).
2.  Start asking your sales data-related questions!

**Example Queries:**

*   **General:**
    *   "Hello, what can you do?"
    *   "What tools do you have access to?"

*   **Google Sheets:**
    *   "Fetch sales data from Google Sheet ID `[YOUR_SPREADSHEET_ID]` range `Sheet1!A1:E50`."
    *   "What is the total revenue from column D in Google Sheet `[YOUR_SPREADSHEET_ID]` range `SalesData!A1:D100`?" (The AI will attempt to use the Google Sheet tool and then the Calculator tool).
    *   "Can you show me the first 5 rows from sheet `[YOUR_SPREADSHEET_ID]` range `Products!A1:C10`?"

*   **PostgreSQL / MySQL:**
    *   "Show me all orders from the 'sales_orders' table in Postgres where the amount is greater than 500."
    *   "What are the top 3 selling products from the 'products' table in MySQL? The table has columns 'product_name' and 'units_sold'." (You might need to guide it to form a `SELECT product_name, units_sold FROM products ORDER BY units_sold DESC LIMIT 3;` query)
    *   "Execute this SQL query on Postgres: `SELECT COUNT(*) FROM customers;`"

*   **Calculator:**
    *   "What is 25 * 4?"
    *   "Calculate the average of 150, 200, and 250."
    *   "If total sales are 12000 and expenses are 3500, what is the profit?"

**Tips for Interacting with the AI:**

*   **Be Specific:** Provide as much detail as possible, especially when referring to Google Sheet IDs, ranges, database table names, and column names. The AI will ask for clarification if needed.
*   **Iterate:** If the first response isn't perfect, try rephrasing your question or providing more context.
*   **Check Tool Usage:** In the n8n execution log, you can see which tools the AI Agent decided to use and what parameters it passed. This is helpful for debugging.

## Tools Reference

*   **Chat Trigger:** Initiates the workflow when a message is received in the n8n chat.
*   **AI Agent:** The "brain" of the chatbot. It processes user input, decides which tool to use, and formulates responses.
*   **Get Google Sheet Data Tool:** Fetches data from a specified Google Sheet.
    *   AI Input: `spreadsheetId`, `range`
*   **Query Postgres DB Tool:** Executes SQL queries on a PostgreSQL database.
    *   AI Input: `query` (SQL string)
*   **Query MySQL DB Tool:** Executes SQL queries on a MySQL database.
    *   AI Input: `query` (SQL string)
*   **Calculator Tool (Math Node):** Performs mathematical calculations.
    *   AI Input: `expression` (mathematical string, e.g., "100 + 50")

## Customization & Extension

*   **Add More Data Sources:** Duplicate existing database/API nodes and configure them for other sources (e.g., SQLite, MSSQL, REST APIs). Then, update the AI Agent's system prompt and tool list.
*   **Refine AI Prompts:** Modify the `System Message` in the AI Agent node to give the AI more specific instructions, context about your data schemas, or a different persona.
*   **Add More Complex Tools:**
    *   Use the **Code Node** to create custom tools that perform more complex logic or data transformations if the AI needs them.
*   **Error Handling:** Add n8n's error handling nodes to manage potential issues during tool execution (e.g., database connection errors, invalid sheet IDs).

## Important Considerations

*   **Credential Security:** Ensure your n8n instance and credentials are secure.
*   **LLM Costs:** Be mindful of the costs associated with using LLM APIs (e.g., OpenAI).
*   **Data Privacy:** Understand where your data is being processed, especially when using cloud-based LLMs.
*   **Query Safety (Databases):** While the AI is instructed to be helpful, always be cautious about the SQL queries it might generate if connected to production databases with write access. It's best to use read-only credentials for the database tools where possible.

---

Happy Analyzing!
