# SQL
#### **Start MySQL in XAMPP**

- Open XAMPP Control Panel → Start MySQL.
    
- Open `phpMyAdmin` at `http://localhost/phpmyadmin`.
    
- Create a database, say: `test_db`.
    
- Create a table, say:

```sql
CREATE TABLE students (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  age INT
);
```

## install following first
```
pip install openai mysql-connector-python python-dotenv
%% create .env  %%
OPENAI_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxxx
MYSQL_HOST=localhost
MYSQL_USER=root
MYSQL_PASSWORD=
MYSQL_DATABASE=test_db

```


```
```Python
import os
import openai
import mysql.connector
from dotenv import load_dotenv

# Load secrets from .env
load_dotenv()

openai.api_key = os.getenv("OPENAI_API_KEY")

# MySQL connection
conn = mysql.connector.connect(
    host=os.getenv("MYSQL_HOST"),
    user=os.getenv("MYSQL_USER"),
    password=os.getenv("MYSQL_PASSWORD"),
    database=os.getenv("MYSQL_DATABASE")
)
cursor = conn.cursor()

def generate_sql_query(user_prompt: str, table_schema: str) -> str:
    """Generate SQL query using GPT"""
    prompt = f"""
    You are an assistant that generates MySQL queries.
    Given the following table schema:
    {table_schema}
    
    User request: "{user_prompt}"
    
    Generate a syntactically correct SQL query.
    """
    
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}],
        temperature=0
    )
    
    return response.choices[0].message["content"].strip()

def execute_query(query: str):
    try:
        cursor.execute(query)
        if query.lower().startswith("select"):
            results = cursor.fetchall()
            for row in results:
                print(row)
        else:
            conn.commit()
            print("Query executed successfully.")
    except mysql.connector.Error as err:
        print(f"Error: {err}")

if __name__ == "__main__":
    table_schema = """
    Table: students
    Columns:
      - id: INT, Primary Key
      - name: VARCHAR(100)
      - age: INT
    """
    
    while True:
        user_input = input("Ask your question (or 'exit'): ")
        if user_input.lower() == "exit":
            break
        sql_query = generate_sql_query(user_input, table_schema)
        print(f"\nGenerated SQL:\n{sql_query}\n")
        execute_query(sql_query)
```


## using langchain agent:-

```
//installation
pip install langchain openai mysql-connector-python python-dotenv

//.env
OPENAI_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxx
MYSQL_HOST=localhost
MYSQL_PORT=3306
MYSQL_USER=root
MYSQL_PASSWORD=
MYSQL_DATABASE=test_db

```

### script
 ```python 
 import os
from langchain.chat_models import ChatOpenAI
from langchain.utilities import SQLDatabase
from langchain.chains import SQLDatabaseChain
from dotenv import load_dotenv

# Load environment variables
load_dotenv()

# MySQL connection string
db = SQLDatabase.from_uri(
    f"mysql+mysqlconnector://{os.getenv('MYSQL_USER')}:{os.getenv('MYSQL_PASSWORD')}@{os.getenv('MYSQL_HOST')}:{os.getenv('MYSQL_PORT')}/{os.getenv('MYSQL_DATABASE')}"
)

# OpenAI LLM
llm = ChatOpenAI(model="gpt-4", temperature=0, openai_api_key=os.getenv("OPENAI_API_KEY"))

# Create the SQL chain
db_chain = SQLDatabaseChain.from_llm(llm, db, verbose=True)

# Use the chain
while True:
    user_input = input("\nAsk your SQL-related question (or type 'exit'): ")
    if user_input.lower() == "exit":
        break
    response = db_chain.run(user_input)
    print("\nAnswer:\n", response)

```


>make sure u have table and database present to fetch query