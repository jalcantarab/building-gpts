Creating a GPT With Custom Knowledge

GPTs are the new Apps for ChatGPT. They are instances of their same models, using the tools available to ChatGPT but with additional tools if you want to give them, files loaded as context for its conversations, and conditioning on how to behave.

This is very useful to create a little swarm of conditioned ChatGPTs that you use in your daily life for different tasks. It can help you go to the next level of AI usage efficiency, avoiding the conversation context you would need to give and the information you would repeatedly need to copy paste in. 

For example, you can have a GPT always generate graphical representatios of data and all you need to do once you've created it is tell it what data you're interested in, and boom it goes straigt to browse information, generate graphs with code interpreter and, if you want, create an image that represents the graph creatively with dall-e. 


For this quick guide, I'm going to build a GPT that will help me build assistants with the OpenAI API. This has information that browsing cannot get, and so I will use retrieval and conditioning to have a GPT that specializes in the OpenAI API. 


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



