# How to call functions with chat models

This notebook covers how to use the Chat Completions API in combination
with external functions to extend the capabilities of GPT models.

`functions` is an optional parameter in the Chat Completion API which
can be used to provide function specifications. The purpose of this is
to enable models to generate function arguments which adhere to the
provided specifications. Note that the API will not actually execute any
function calls. It is up to developers to execute function calls using
model outputs.

If the `functions` parameter is provided then by default the model will
decide when it is appropriate to use one of the functions. The API can
be forced to use a specific function by setting the `function_call`
parameter to `{"name": "<insert-function-name>"}`. The API can also be
forced to not use any function by setting the `function_call` parameter
to `"none"`. If a function is used, the output will contain
`"finish_reason": "function_call"` in the response, as well as a
`function_call` object that has the name of the function and the
generated function arguments.

### Overview

This notebook contains the following 2 sections:

-   **How to generate function arguments:** Specify a set of functions
    and use the API to generate function arguments.
-   **How to call functions with model generated arguments:** Close the
    loop by actually executing functions with model generated arguments.
:::

::: {#64c85e26 .cell .markdown}
## How to generate function arguments
:::

::: {#80e71f33 .cell .code pycharm="{\"is_executing\":true}"}
``` python
!pip install scipy
!pip install tenacity
!pip install tiktoken
!pip install termcolor 
!pip install openai
!pip install requests
```
:::

::: {#dab872c5 .cell .code execution_count="1"}
``` python
import json
import openai
import requests
from tenacity import retry, wait_random_exponential, stop_after_attempt
from termcolor import colored

GPT_MODEL = "gpt-3.5-turbo-0613"
```
:::

::: {#69ee6a93 .cell .markdown}
### Utilities

First let\'s define a few utilities for making calls to the Chat
Completions API and for maintaining and keeping track of the
conversation state.
:::

::: {#745ceec5 .cell .code execution_count="2"}
``` python
@retry(wait=wait_random_exponential(multiplier=1, max=40), stop=stop_after_attempt(3))
def chat_completion_request(messages, functions=None, function_call=None, model=GPT_MODEL):
    headers = {
        "Content-Type": "application/json",
        "Authorization": "Bearer " + openai.api_key,
    }
    json_data = {"model": model, "messages": messages}
    if functions is not None:
        json_data.update({"functions": functions})
    if function_call is not None:
        json_data.update({"function_call": function_call})
    try:
        response = requests.post(
            "https://api.openai.com/v1/chat/completions",
            headers=headers,
            json=json_data,
        )
        return response
    except Exception as e:
        print("Unable to generate ChatCompletion response")
        print(f"Exception: {e}")
        return e
```
:::

::: {#c4d1c99f .cell .code execution_count="3"}
``` python
def pretty_print_conversation(messages):
    role_to_color = {
        "system": "red",
        "user": "green",
        "assistant": "blue",
        "function": "magenta",
    }
    
    for message in messages:
        if message["role"] == "system":
            print(colored(f"system: {message['content']}\n", role_to_color[message["role"]]))
        elif message["role"] == "user":
            print(colored(f"user: {message['content']}\n", role_to_color[message["role"]]))
        elif message["role"] == "assistant" and message.get("function_call"):
            print(colored(f"assistant: {message['function_call']}\n", role_to_color[message["role"]]))
        elif message["role"] == "assistant" and not message.get("function_call"):
            print(colored(f"assistant: {message['content']}\n", role_to_color[message["role"]]))
        elif message["role"] == "function":
            print(colored(f"function ({message['name']}): {message['content']}\n", role_to_color[message["role"]]))
```
:::

::: {#29d4e02b .cell .markdown}
### Basic concepts

Let\'s create some function specifications to interface with a
hypothetical weather API. We\'ll pass these function specification to
the Chat Completions API in order to generate function arguments that
adhere to the specification.
:::

::: {#d2e25069 .cell .code execution_count="4"}
``` python
functions = [
    {
        "name": "get_current_weather",
        "description": "Get the current weather",
        "parameters": {
            "type": "object",
            "properties": {
                "location": {
                    "type": "string",
                    "description": "The city and state, e.g. San Francisco, CA",
                },
                "format": {
                    "type": "string",
                    "enum": ["celsius", "fahrenheit"],
                    "description": "The temperature unit to use. Infer this from the users location.",
                },
            },
            "required": ["location", "format"],
        },
    },
    {
        "name": "get_n_day_weather_forecast",
        "description": "Get an N-day weather forecast",
        "parameters": {
            "type": "object",
            "properties": {
                "location": {
                    "type": "string",
                    "description": "The city and state, e.g. San Francisco, CA",
                },
                "format": {
                    "type": "string",
                    "enum": ["celsius", "fahrenheit"],
                    "description": "The temperature unit to use. Infer this from the users location.",
                },
                "num_days": {
                    "type": "integer",
                    "description": "The number of days to forecast",
                }
            },
            "required": ["location", "format", "num_days"]
        },
    },
]
```
:::

::: {#bfc39899 .cell .markdown}
If we prompt the model about the current weather, it will respond with
some clarifying questions.
:::

::: {#518d6827 .cell .code execution_count="5"}
``` python
messages = []
messages.append({"role": "system", "content": "Don't make assumptions about what values to plug into functions. Ask for clarification if a user request is ambiguous."})
messages.append({"role": "user", "content": "What's the weather like today"})
chat_response = chat_completion_request(
    messages, functions=functions
)
assistant_message = chat_response.json()["choices"][0]["message"]
messages.append(assistant_message)
assistant_message
```

::: {.output .execute_result execution_count="5"}
    {'role': 'assistant',
     'content': 'In which city and state would you like to know the current weather?'}
:::
:::

::: {#4c999375 .cell .markdown}
Once we provide the missing information, it will generate the
appropriate function arguments for us.
:::

::: {#23c42a6e .cell .code execution_count="6"}
``` python
messages.append({"role": "user", "content": "I'm in Glasgow, Scotland."})
chat_response = chat_completion_request(
    messages, functions=functions
)
assistant_message = chat_response.json()["choices"][0]["message"]
messages.append(assistant_message)
assistant_message
```

::: {.output .execute_result execution_count="6"}
    {'role': 'assistant',
     'content': None,
     'function_call': {'name': 'get_current_weather',
      'arguments': '{\n  "location": "Glasgow, Scotland",\n  "format": "celsius"\n}'}}
:::
:::

::: {#c14d4762 .cell .markdown}
By prompting it differently, we can get it to target the other function
we\'ve told it about.
:::

::: {#fa232e54 .cell .code execution_count="7"}
``` python
messages = []
messages.append({"role": "system", "content": "Don't make assumptions about what values to plug into functions. Ask for clarification if a user request is ambiguous."})
messages.append({"role": "user", "content": "what is the weather going to be like in Glasgow, Scotland over the next x days"})
chat_response = chat_completion_request(
    messages, functions=functions
)
assistant_message = chat_response.json()["choices"][0]["message"]
messages.append(assistant_message)
assistant_message
```

::: {.output .execute_result execution_count="7"}
    {'role': 'assistant',
     'content': 'Sure, I can help you with that. Please provide me with the number of days you want to forecast for.'}
:::
:::

::: {#6172ddac .cell .markdown}
Once again, the model is asking us for clarification because it doesn\'t
have enough information yet. In this case it already knows the location
for the forecast, but it needs to know how many days are required in the
forecast.
:::

::: {#c7d8a543 .cell .code execution_count="8"}
``` python
messages.append({"role": "user", "content": "5 days"})
chat_response = chat_completion_request(
    messages, functions=functions
)
chat_response.json()["choices"][0]
```

::: {.output .execute_result execution_count="8"}
    {'index': 0,
     'message': {'role': 'assistant',
      'content': None,
      'function_call': {'name': 'get_n_day_weather_forecast',
       'arguments': '{\n  "location": "Glasgow, Scotland",\n  "format": "celsius",\n  "num_days": 5\n}'}},
     'finish_reason': 'function_call'}
:::
:::

::: {#4b758a0a .cell .markdown}
#### Forcing the use of specific functions or no function
:::

::: {#412f79ba .cell .markdown}
We can force the model to use a specific function, for example
get_n\_day_weather_forecast by using the function_call argument. By
doing so, we force the model to make assumptions about how to use it.
:::

::: {#559371b7 .cell .code execution_count="9"}
``` python
# in this cell we force the model to use get_n_day_weather_forecast
messages = []
messages.append({"role": "system", "content": "Don't make assumptions about what values to plug into functions. Ask for clarification if a user request is ambiguous."})
messages.append({"role": "user", "content": "Give me a weather report for Toronto, Canada."})
chat_response = chat_completion_request(
    messages, functions=functions, function_call={"name": "get_n_day_weather_forecast"}
)
chat_response.json()["choices"][0]["message"]
```

::: {.output .execute_result execution_count="9"}
    {'role': 'assistant',
     'content': None,
     'function_call': {'name': 'get_n_day_weather_forecast',
      'arguments': '{\n  "location": "Toronto, Canada",\n  "format": "celsius",\n  "num_days": 1\n}'}}
:::
:::

::: {#a7ab0f58 .cell .code execution_count="10"}
``` python
# if we don't force the model to use get_n_day_weather_forecast it may not
messages = []
messages.append({"role": "system", "content": "Don't make assumptions about what values to plug into functions. Ask for clarification if a user request is ambiguous."})
messages.append({"role": "user", "content": "Give me a weather report for Toronto, Canada."})
chat_response = chat_completion_request(
    messages, functions=functions
)
chat_response.json()["choices"][0]["message"]
```

::: {.output .execute_result execution_count="10"}
    {'role': 'assistant',
     'content': None,
     'function_call': {'name': 'get_current_weather',
      'arguments': '{\n  "location": "Toronto, Canada",\n  "format": "celsius"\n}'}}
:::
:::

::: {#3bd70e48 .cell .markdown}
We can also force the model to not use a function at all. By doing so we
prevent it from producing a proper function call.
:::

::: {#acfe54e6 .cell .code execution_count="11"}
``` python
messages = []
messages.append({"role": "system", "content": "Don't make assumptions about what values to plug into functions. Ask for clarification if a user request is ambiguous."})
messages.append({"role": "user", "content": "Give me the current weather (use Celcius) for Toronto, Canada."})
chat_response = chat_completion_request(
    messages, functions=functions, function_call="none"
)
chat_response.json()["choices"][0]["message"]
```

::: {.output .execute_result execution_count="11"}
    {'role': 'assistant', 'content': 'Sure, let me get that information for you.'}
:::
:::

::: {#b4482aee .cell .markdown}
## How to call functions with model generated arguments

In our next example, we\'ll demonstrate how to execute functions whose
inputs are model-generated, and use this to implement an agent that can
answer questions for us about a database. For simplicity we\'ll use the
[Chinook sample
database](https://www.sqlitetutorial.net/sqlite-sample-database/).

*Note:* SQL generation can be high-risk in a production environment
since models are not perfectly reliable at generating correct SQL.
:::

::: {#f7654fef .cell .markdown}
### Specifying a function to execute SQL queries

First let\'s define some helpful utility functions to extract data from
a SQLite database.
:::

::: {#30f6b60e .cell .code execution_count="12"}
``` python
import sqlite3

conn = sqlite3.connect("data/Chinook.db")
print("Opened database successfully")
```

::: {.output .stream .stdout}
    Opened database successfully
:::
:::

::: {#abec0214 .cell .code execution_count="13"}
``` python
def get_table_names(conn):
    """Return a list of table names."""
    table_names = []
    tables = conn.execute("SELECT name FROM sqlite_master WHERE type='table';")
    for table in tables.fetchall():
        table_names.append(table[0])
    return table_names


def get_column_names(conn, table_name):
    """Return a list of column names."""
    column_names = []
    columns = conn.execute(f"PRAGMA table_info('{table_name}');").fetchall()
    for col in columns:
        column_names.append(col[1])
    return column_names


def get_database_info(conn):
    """Return a list of dicts containing the table name and columns for each table in the database."""
    table_dicts = []
    for table_name in get_table_names(conn):
        columns_names = get_column_names(conn, table_name)
        table_dicts.append({"table_name": table_name, "column_names": columns_names})
    return table_dicts
```
:::

::: {#77e6e5ea .cell .markdown}
Now can use these utility functions to extract a representation of the
database schema.
:::

::: {#0c0104cd .cell .code execution_count="14"}
``` python
database_schema_dict = get_database_info(conn)
database_schema_string = "\n".join(
    [
        f"Table: {table['table_name']}\nColumns: {', '.join(table['column_names'])}"
        for table in database_schema_dict
    ]
)
```
:::

::: {#ae73c9ee .cell .markdown}
As before, we\'ll define a function specification for the function we\'d
like the API to generate arguments for. Notice that we are inserting the
database schema into the function specification. This will be important
for the model to know about.
:::

::: {#0258813a .cell .code execution_count="15"}
``` python
functions = [
    {
        "name": "ask_database",
        "description": "Use this function to answer user questions about music. Input should be a fully formed SQL query.",
        "parameters": {
            "type": "object",
            "properties": {
                "query": {
                    "type": "string",
                    "description": f"""
                            SQL query extracting info to answer the user's question.
                            SQL should be written using this database schema:
                            {database_schema_string}
                            The query should be returned in plain text, not in JSON.
                            """,
                }
            },
            "required": ["query"],
        },
    }
]
```
:::

::: {#da08c121 .cell .markdown}
### Executing SQL queries

Now let\'s implement the function that will actually excute queries
against the database.
:::

::: {#65585e74 .cell .code execution_count="16"}
``` python
def ask_database(conn, query):
    """Function to query SQLite database with a provided SQL query."""
    try:
        results = str(conn.execute(query).fetchall())
    except Exception as e:
        results = f"query failed with error: {e}"
    return results

def execute_function_call(message):
    if message["function_call"]["name"] == "ask_database":
        query = json.loads(message["function_call"]["arguments"])["query"]
        results = ask_database(conn, query)
    else:
        results = f"Error: function {message['function_call']['name']} does not exist"
    return results
```
:::

::: {#38c55083 .cell .code execution_count="17"}
``` python
messages = []
messages.append({"role": "system", "content": "Answer user questions by generating SQL queries against the Chinook Music Database."})
messages.append({"role": "user", "content": "Hi, who are the top 5 artists by number of tracks?"})
chat_response = chat_completion_request(messages, functions)
assistant_message = chat_response.json()["choices"][0]["message"]
messages.append(assistant_message)
if assistant_message.get("function_call"):
    results = execute_function_call(assistant_message)
    messages.append({"role": "function", "name": assistant_message["function_call"]["name"], "content": results})
pretty_print_conversation(messages)
```

::: {.output .stream .stdout}
    system: Answer user questions by generating SQL queries against the Chinook Music Database.

    user: Hi, who are the top 5 artists by number of tracks?

    assistant: {'name': 'ask_database', 'arguments': '{\n  "query": "SELECT ar.Name, COUNT(t.TrackId) AS NumTracks FROM Artist ar INNER JOIN Album al ON ar.ArtistId = al.ArtistId INNER JOIN Track t ON al.AlbumId = t.AlbumId GROUP BY ar.ArtistId ORDER BY NumTracks DESC LIMIT 5"\n}'}

    function (ask_database): [('Iron Maiden', 213), ('U2', 135), ('Led Zeppelin', 114), ('Metallica', 112), ('Lost', 92)]
:::
:::

::: {#710481dc .cell .code execution_count="18" scrolled="true"}
``` python
messages.append({"role": "user", "content": "What is the name of the album with the most tracks?"})
chat_response = chat_completion_request(messages, functions)
assistant_message = chat_response.json()["choices"][0]["message"]
messages.append(assistant_message)
if assistant_message.get("function_call"):
    results = execute_function_call(assistant_message)
    messages.append({"role": "function", "content": results, "name": assistant_message["function_call"]["name"]})
pretty_print_conversation(messages)
```

::: {.output .stream .stdout}
    system: Answer user questions by generating SQL queries against the Chinook Music Database.

    user: Hi, who are the top 5 artists by number of tracks?

    assistant: {'name': 'ask_database', 'arguments': '{\n  "query": "SELECT ar.Name, COUNT(t.TrackId) AS NumTracks FROM Artist ar INNER JOIN Album al ON ar.ArtistId = al.ArtistId INNER JOIN Track t ON al.AlbumId = t.AlbumId GROUP BY ar.ArtistId ORDER BY NumTracks DESC LIMIT 5"\n}'}

    function (ask_database): [('Iron Maiden', 213), ('U2', 135), ('Led Zeppelin', 114), ('Metallica', 112), ('Lost', 92)]

    user: What is the name of the album with the most tracks?

    assistant: {'name': 'ask_database', 'arguments': '{\n  "query": "SELECT al.Title, COUNT(t.TrackId) AS NumTracks FROM Album al INNER JOIN Track t ON al.AlbumId = t.AlbumId GROUP BY al.AlbumId ORDER BY NumTracks DESC LIMIT 1"\n}'}

    function (ask_database): [('Greatest Hits', 57)]
:::
:::

::: {#2d89073c .cell .markdown}
## Next Steps

See our other
[notebook](How_to_call_functions_for_knowledge_retrieval.ipynb) that
demonstrates how to use the Chat Completions API and functions for
knowledge retrieval to interact conversationally with a knowledge base.
:::
