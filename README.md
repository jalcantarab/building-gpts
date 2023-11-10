# What Are GPTs?

**_GPTs_**  are akin to apps for the ChatGPT user platform, designed to operate with a specific set of  _skills_,  _knowledge_, and  _behaviors_. They are personalized instances of the larger models, that can be equipped with custom tools and data, and conditioned to manage the conversation fitting your specific requirements.

They are designed to act as a personal cadre of AI assistants, each programmed to understand and execute tasks with a level of precision that you cannot achieve without a lot of pre-prompting in a normal ChatGPT conversation.

Although similar tools exist in the market, none has had the chance to integrate with the other updates that OpenAI released on the same day. With  `gpt-4-turbo`, and a new knowledge cutoff of April 2023,  **_GPTs_**  were instantly on launch, the most capable platform for no-code  _supervised_  AI development.

## Why GPTs Matter

This tailored approach to AI utility offers substantial benefits, particularly in streamlining task execution. By eliminating the need for repetitive context-setting and data input,  **_GPTs_**  free users to engage with more complex, value-added activities. They pave the way for advancements in how we interact with data, interpret insights, and make decisions.

It is a bet that OpenAI is making to move us to a marketplace model for AI apps, where users create useful final products that OpenAI profits from. They made an initial attempt with Plugins, but never moved to create a real market for it.

For instance, a  **_GPT_**  designed for data visualization can immediately interpret a user’s request, sift through uploaded information repositories, generate precise graphical outputs, and, if instructed, create an imaginative visual representation using the Dall-E integration. The result is a seamless bridge from raw data to actionable and visually impactful information.

It follows, of course, that the introduction of  **_GPTs_**  represents a significant evolution from the traditional use of customer AI. By allowing users to load context-specific files and set behavioral conditions,  **_GPTs_**  really make focused and personalized AI experiences available to users.

## Yes, yes, but what are they capable of?

First, what it cannot do (at the moment).  **_GPTs_**  are not autonomous and expect to interact with the user in a conversation. OpenAI openly said this is preparing the market for semi-autonomous AI.

![](https://miro.medium.com/v2/resize:fit:700/0*6laVIepTZyS31IRF)

Image generated using GPT-4 and DALL-E 3

And still, without that autonomy, it can do wonders. It can chain multiple actions (_steps_) within a response (_run_) when conditioned correctly. An example of this could be:

1.  Our Custom GPT receives a user request to visualize some data in an abstract way
2.  The GPT pulls relevant data from the files it has indexed to understand what the user is referring to.
3.  It understands it but it is also conditioned to get up-to-date information. So it makes one or multiple Bing searches based on the concepts it found on the files.
4.  Assuming it gets the data it looked for, it then writes and runs its own code to analyze the downloaded online data, generates plots, transforms the results, or even…
5.  It sends the results to Dall-E to generate the abstract image requested based on the interpreted, processed, and scraped data.

> It is not a breakthrough in technology–they’ve already done that throughout the year–, but a very elegant application of their tech that brings AI closer to users at any level of technical curiosity. It is a tool to increase efficiency on repeat tasks.  **And a very good one at that.**

![](https://miro.medium.com/v2/resize:fit:700/0*dASZZ7eqKIP0B95S)

La tienda de GPTs –  _By DALL-E_

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

So, share away or keep it to yourself, but you are now ready to start using this  **_GPT_**  and start embedding abstract data representations in all your decks!

## Let’s see it in action

I have a seminar where we need to present to compliance executives the need to prepare for the procurement of AI tools. I would love to have some fun images to take the weight off the conversation but point to the argument that ChatGPT is already in their employee's household. Let’s see how our  **_Visual Synthesizer_**  can help.

> Let’s create an image that shows houses of or varying size that match the relative amount of users that ChatGPT has had over time since Nov 2022 until nov 2023

![](https://miro.medium.com/v2/resize:fit:700/1*xsMc6rOXsEf66vmfXaGwpw.png)

It will not always use all of its tools, and it doesn’t need to, but you can condition to use what you prefer in the chat like a normal ChatGPT, but without needing to prime it or remind it constantly what its objective is.

I want another image to go with the slide where we cover the different models and their properties.

> Let’s create an image that shows brains of varying sizes relative to the sizes of the OpenAI, Palm, Llama & Anthropic models. Make the necessary online searches until information is found. If any model cannot be found, take it out of consideration.

![](https://miro.medium.com/v2/resize:fit:700/1*ZkbJ7rGija4VQJbCFjljCw.png)

Feel free to try  [this same  **_GPT_**](https://chat.openai.com/g/g-MhYABMY0f-visual-synthesizer)  or build one following these instructions. In any case, happy automating! 
