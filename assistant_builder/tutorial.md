Creating a GPT With Custom Knowledge

- Browsing may be sufficient if the pages you need to access do not load using JS or have any other limit preventing Bing browsing from accessing them. 

- If you can't access them, you need to download their content. 
- - A user-level way to do this is simply to download the content of the pages and upload it to the GPT. 

- - If you want a programatic way, right now you would need to use Assistants rather than GPTs. 

In this example, I know I want help working with the newer Assistants API and function calling, which has a little bit more information available. 

So for this purpose, I prepared my GPT with the Guides from OpenAI & a couple cookbook examples from their github
https://platform.openai.com/docs/assistants/overview
https://platform.openai.com/docs/assistants/how-it-works
https://platform.openai.com/docs/assistants/tools

https://github.com/openai/openai-cookbook/blob/main/examples/How_to_call_functions_for_knowledge_retrieval.ipynb
https://github.com/openai/openai-cookbook/blob/main/examples/How_to_call_functions_with_chat_models.ipynb


RAG only accepts some type of files, so we placed all the content from the first three links into a single markdown file (.md), but you can just download the HTML, or copy the text. In our case, we wanted to keep the information from tables and the structure, but did not care abou the style. So we use markdown, which saves all the information we need whilst consuming the least tokens. 


For assistant use-cases where you're not looking to run code, it is better to turn it off to avoid the conversation getting derailed. 

How to add calls to functions (Actions) within GPTs: 



