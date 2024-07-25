

---

# SQL to Spark SQL/PySpark Translator for Azure Databricks

The Translator.ipynb notebook assists in translating T-SQL stored procedures to Spark SQL for use in Azure Databricks. It validates input, identifies dependencies, and generates Spark SQL/PySpark code with the assistance of gpt-4o. <br>
Additional documentation can be found under documentation/translator_short_deck.pdf.

## How to Use / Configuration

### Prerequisites
- Ensure you have access to an Azure OpenAI subscription with a GPT-4o model provisioned. Increase the token per minute limit (TPM) to at least 130K.
- Duplicate the `.env.sample` file and rename it to `.env` before populating it with the necessary credentials and configurations.
- Install the necessary libraries by running the following command in a Jupyter cell:
  ```python
  %pip install pandas sqlglot openai networkx matplotlib python-dotenv tiktoken
  ```

### Configuration
1. **Environment Variables**: Populate the `.env` file with the required environment variables:
   - `AZURE_OPENAI_ENDPOINT`
   - `AZURE_OPENAI_API_KEY`
   - `AZURE_OPENAI_PREVIEW_API_VERSION`
   - `AZURE_OPENAI_MODEL_NAME`
   - `AZURE_OPENAI_TEMPERATURE`
   - `AZURE_OPENAI_MAX_TOKENS`
2. **Load the Dependencies**: Run the cell to load necessary libraries and environment variables.

## Component by Component Walk-Through

### 1. **Introduction and Setup**
   - Provides an overview of the notebook's purpose and lists prerequisites.

### 2. **Load Libraries and Environment Variables**
   - This cell loads the required libraries and environment variables from the `.env` file:
     ```python
     import os
     import pandas
     import networkx
     import matplotlib
     from IPython.display import Markdown, display
     from dotenv import load_dotenv

     load_dotenv()
     ```

### 3. **Define GPT-4o Query Function**
   - Defines the function to query GPT-4o for translation:
     ```python
     from openai import AzureOpenAI

     client = AzureOpenAI(
         azure_endpoint = os.getenv("AZURE_OPENAI_ENDPOINT"),
         api_key = os.getenv("AZURE_OPENAI_API_KEY"),
         api_version = os.getenv("AZURE_OPENAI_PREVIEW_API_VERSION"),
     )

     temperature = float(os.getenv("AZURE_OPENAI_TEMPERATURE"))
     max_tokens = os.getenv("AZURE_OPENAI_MAX_TOKENS")

     def gpt_4o_analysis(system_message, user_query):
         response = client.chat.completions.create(
             model=os.getenv("AZURE_OPENAI_MODEL_NAME"),
             messages=[
                 {"role": "system", "content": system_message},
                 {"role": "user", "content": f"The user's query is: {user_query}"}
             ],
             temperature=temperature
         )
         return response
     ```

### 4. **Define Translation Function**
   - The main function to translate T-SQL to Spark SQL using GPT-4o:
     ```python
     def translate_to_spark_sql(tsql):
         system_message = "You are an AI assistant that helps translate T-SQL to PySpark."
         user_query = """..."""  # Detailed instructions for the model
         result = gpt_4o_analysis(system_message, "Your goal is to translate the following T-SQL query to PySpark...")
         return result
     ```

### 5. **Main Execution Logic**
   - Checks if the script should read from a file or from a direct input, processes the translation, and displays the results:
     ```python
     if not read_from_file:
         spark_sql_procedure = translate_to_spark_sql(tsql_procedure)
         print("Translated PySpark:")
         display(Markdown(spark_sql_procedure))
     else:
         for query in ddl_sql_list:
             spark_sql_query = translate_to_spark_sql(query)
             display(Markdown(f"### Original Query: \n\n```sql\n{query}\n```\n\n"))
             print("Translated PySpark:")
             display(Markdown(spark_sql_query))
             ...
     ```

## Use-Cases for this Translator

### 1. **Migrating Legacy Systems to Azure Databricks**
   - Translate existing T-SQL stored procedures to Spark SQL for seamless migration to Azure Databricks.

### 2. **Optimizing Query Performance**
   - Convert T-SQL scripts to Spark SQL to leverage the distributed computing capabilities of Spark for improved performance.

### 3. **Data Processing and ETL Pipelines**
   - Use the translated Spark SQL queries in your data processing and ETL pipelines within Azure Databricks for efficient data management.

### 4. **Consistency in Data Handling**
   - Maintain consistent data handling practices across different environments by translating SQL queries to Spark SQL for use in Azure Databricks.

### 5. **Educational and Training Purposes**
   - Demonstrate the process of migrating SQL queries to Spark SQL in educational settings or training programs focused on cloud data solutions.

---

Individual Technical Specialist Intern Project / Summer 2024, Microsoft Canada Data & AI STU/CSU