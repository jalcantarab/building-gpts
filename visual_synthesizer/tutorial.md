# Creating Your Own GPTs: Step-by-Step

Creating a GPT is simpler than you might think. With OpenAI’s new User Interface, you can start by just chatting with ChatGPT! It will ask you a series of questions to define the main behavior (_Instructions_), its name, and an image.

![](https://miro.medium.com/v2/resize:fit:700/1*EiUJMLJtaxWc1iThk4Ddag.png)

## Step 1 — Configuration

But really, at the moment you do need some extra work to really get the most out of this tool. Once you’ve set up your first  **_GPTs_**  you may prefer to go straight to the  `Configure`  tab and build over what the chat just created.

-   **Name:**  You will want something short, that can show on the sidebar. You will also want to use this name within the instructions, so the shorter and more descriptive the better.
-   **Description:**  Only used for external human purposes.
-   **Instructions:**  This is like your system prompt for the  **_GPT_**. Here you define its behavior. You will want to include the actions to take, the resources to use, the style, tone, and general objectives.  **This is the most important section of the configuration.**
-   **Conversation Starters:**  The helper suggestions that appear on top of your chat input. Provides users with easy prompts to get started.
-   **Knowledge:**  Here is where you upload the files you want the  **_GPT_**  to use in all sessions, either for Retrieval or Code Interpreter.
-   **Capabilities:** Off-the-shelf powerful tools you can give your  **_GPT_**. We will cover them in more detail.
-   **Actions:**  This is where you can define calls to external services that the  **_GPT_**  may use in the course of writing its answers.

![](https://miro.medium.com/v2/resize:fit:700/1*3wLLSdGzzsRfHo72VGM8Bg.png)

## Step 2 — Finding and Indexing the Right Knowledge

For each use case, you will need a different knowledge corpus. Today, OpenAI’s limit is 20 files, 500MB each max. But it already shows low performance with files even half that long, so you must be critical of what knowledge to load in, and you may need to preprocess it for key use cases. Knowledge files will be used in two possible ways:

-   **Retrieval:** Some file types, particularly text, pdf, html…
-   **Code Interpreter:** Most file types can be read and used by CI.

What I’m doing for these initial  **_GPTs_**  is converting relevant documentation into text or markdown files so that the  **_GPT_**  has all the necessary information without using too many tokens in context later. This preparation step is crucial for a well-informed assistant.

**_Sources –_** Choose the most reliable and comprehensive sources for the topic at hand. For development copilots, official documentation and sanctioned guides are your best bet. For creative work, varied public sources.

**_Formats –_** Not all file formats are created equal. For retrieval purposes, your  **_GPT_**  will work best with text and markdown files, which are easily indexed and consume fewer tokens.

Once formatted, upload your files to the  **_GPT_**’s knowledge base. Make sure to maintain a logical structure that mirrors how the information is likely to be retrieved during a conversation.

**_We are nearly done!!_**

## Step 3 — Refining Your GPT’s Instructions

The instructions are your  **_GPT’s_**  roadmap to an answer, guiding it on how and when to use its tools and how to write back to the user. Clear and precise instructions are crucial for ensuring that your  **_GPT_**  performs optimally.

**Defining Objectives:**  Start with a clear statement of what you want your  **_GPT_**  to achieve in its interactions.

**Specifying Tools and Resources:**  Clearly delineate which tools the  **_GPT_**  should use (and  _when_), such as Bing for updated information or the code interpreter for data analysis. This applies as well to any Action that you add in, you will need to tell your  **_GPT_**  in the instructions how to use it.

**Setting the Tone and Style:** Decide how formal or casual you want your GPT to be, and whether it should adopt a particular persona.

![](https://miro.medium.com/v2/resize:fit:700/1*PJRI0MIAURagkbLasA9lvw.png)

> **Craft Clear Instructions.**  Once you’ve completed a round of additions make sure to review and summarize. Are you repeating yourself? Is any phrase out of place?

## Step 4 — Enabling Advanced Capabilities

Determine which of the off-the-shelf services, like Bing search or DALL-E, your  **_GPT_**  will call upon. Do not activate all of them if you don’t need to, as  **_GPTs_**  seem to have a tendency to attempt to use enabled tools. Code Interpreter in particular should be disabled unless you expect it to need to run its own code.

Actions can extend the capabilities way beyond the ChatGPT interface, so I will leave them for another article.

> **Test and Iterate.**  Use a dataset to test your  **GPT**’s capabilities. Adjust the instructions, actions, and knowledge base as needed based on the outputs until you are happy with the result.

## Step 5 —Save & Publish

Once you are happy with how your GPT is responding to your questions, you can click on the top-right button to Save the  **_GPT_**. This will give you three options:

![](https://miro.medium.com/v2/resize:fit:502/1*v_Bd2nQx0AxSRkJ0LQYNYg.png)

1.  **Only me –** Self-explanatory.
2.  **Only people with a link –** The GPT is public but not broadcasted, if you don’t share it, no one should know about it.
3.  **Public –** It will be available for discovery (details to come).
