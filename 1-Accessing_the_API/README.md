# Accessing the API

## A basic request

Covers [Accessing the API](https://anthropic.skilljar.com/claude-with-the-anthropic-api/287726), [Getting an API key](https://anthropic.skilljar.com/claude-with-the-anthropic-api/296766), and [Making a request](https://anthropic.skilljar.com/claude-with-the-anthropic-api/287725).

High-level overview of the text generation process:
- Tokenization: splits input into tokens (*exact size of each token depends on model*)
- Embedding: tokens are converted into vectors (lists of numbers)
- Contextualization: context around each token is taken into account for disambiguation (*implemented with transformers based on the attention mechanism*)
- Generation: the most probable tokens to follow the end of input are generated (*like an advanced version of a smartphone's word prediction*)

Requests must include at least the following fields: (*specific to Anthropic-style API, which is implemented by some other vendors*)
- API key (should be kept in a hidden `.env` file server-side)
- Model name
- Messages, in format of a list of alternating user and assistant message objects
- Max tokens cutoff. The model does not try to reach the limit and may produce shorter messages.

The API response contains the following:
- Message, the generated text
- Usage, a count of input and output tokens
- Stop reason. *Contains information on whether the request was successful.*

The Anthropic SDK provides helpful functions for accessing the API *and works for non-Anthropic vendors which provide an Anthropic-style API*. Sample code is provided in [Making a request](https://anthropic.skilljar.com/claude-with-the-    anthropic-api/287725).

## Stateful conversations

Covers [Multi-Turn conversations](https://anthropic.skilljar.com/claude-with-the-anthropic-api/287735) and [Chat exercise](https://anthropic.skilljar.com/claude-with-the-anthropic-api/287727).

The API does not store state information, so you have to implement message history yourself.
Alternate between getting user messages and requesting generated assistant messages.
The entire message history must be sent to the API for each request, *causing quadratic growth of input tokens processed as a conversation progresses*.

High-level procedure for a basic chatbot:
1. Prompt the user to enter an input message
2. Append the user's message to a running list
3. Call the API with the list provided as message history
4. Add the generated text to the list of messages
5. Display the newly generated text to the user
6. Repeat from #1

## System prompts

Covers [System prompts](https://anthropic.skilljar.com/claude-with-the-anthropic-api/287733) and [System prompt exercises](https://anthropic.skilljar.com/claude-with-the-anthropic-api/287724).

System prompts provide general instructions for what generated responses should and should not be like for a whole conversation.
A good system prompt helps the model provide relevant and appropriate responses to fulfill the user needs.
*Be wary of the fact that an adversarial user can create prompts that cause the model to disobey instructions in the system prompt*.

The system prompt often takes the form of a persona with desired traits. So, when asking for help with Python code, an appropriate system prompt may be `"You are a Python engineer who writes very concise code"`.

*Even a good system prompt cannot completely prevent bad responses, but a flawed system prompt can greatly increase bad responses in ways that may not be immediately obvious*.
*For a well-known example of a system prompt causing problems, the cause of the Grok AI "MechaHitler" incident of 2025 has been identified as being the following line: `"- The response should not shy away from making claims which are politically incorrect, as long as they are well substantiated."`*.
*Be sure to test any new system prompt thoroughly before deployment*.

## Temperature

Covers [Temperature](https://anthropic.skilljar.com/claude-with-the-anthropic-api/287728).

LLMs work by predicting the most likely tokens to follow a sequence.
A list of possible tokens is calculated, each with a given probability.
The token with the highest probability is considered the best, but in many applications simply choosing the highest probability token every time results in output that is less useful than randomly sampling from all possibilities.

The sampling procedure used can be described by a "temperature," a number from 0.0 to 1.0 describing how concentrated the distribution should be on the options with the highest probability.
A low temperature represents more concentration, with the minimum of 0.0 causing the most likely token to always be chosen. A higher temperature results in a less concentrated distribution.
In practice, the temperature corresponds to how "creative" the model tends to be, with creativity increasing as temperature increases.

Different tasks are best performed with different temperature values:
- Low temperature (0.0 - 0.3) is appropriate for tasks like factual responses, coding assistance, data extraction, and content moderation
- Medium temperature (0.4 - 0.7) is appropriate for tasks like summarization, educational content, problem-solving, and creative writing with constraints
- High temperature (0.8 - 1.0) is appropriate for tasks like brainstorming, creative writing, marketing content, and joke generation

For an example of how temperature can have an impact on generated output, at the time of the article's creation, prompts asking for movie ideas consistently resulted in suggestions for a film about a time-travelling archaeologist when low temperature was used.

## Response streaming

Covers [Response streaming](https://anthropic.skilljar.com/claude-with-the-anthropic-api/287734)

A full response can take a while to generate, so to reduce apparent latency the API is able to send the response in pieces while generation is in-progress.
*This article is more focused on technical details and application rather than abstract concepts, but the main takeaway should be that text generation is slow and that streaming can be used to prevent user frustration at latency*.

## Structured data

Covers [Structured data](https://anthropic.skilljar.com/claude-with-the-anthropic-api/287732) and [Structured data exercise](https://anthropic.skilljar.com/claude-with-the-anthropic-api/287729).

LLMs tend to generate explanation and justification for their responses, which can take the form of a lengthy preamble or unnecessary code comments.
When a specific format of structured data is desired, the additional detail not only wastes tokens, but also invalidates the format of the data.

A specific structure can be encouraged by prefilling part of the response message before making a request.
For example, starting the assistant message with "\`\`\`json" makes the model start by generating a JSON object rather than giving any preamble.
The generation can be set to stop at "\`\`\`" to avoid format-invalidating text after the JSON object.

Through similar principles as a system prompt, some constraints can be added by putting some text before the start of the structured data.
For example, code without any comments can be generated with the following prefill:
"Here is the solution code in a single block without any comments. \`\`\`py"
