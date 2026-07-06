# Accessing the API

## A basic request

Covers [Accessing the API](https://anthropic.skilljar.com/claude-with-the-anthropic-api/287726), [Getting an API key](https://anthropic.skilljar.com/claude-with-the-anthropic-api/296766), and [Making a request](https://anthropic.skilljar.com/claude-with-the-anthropic-api/287725).

High-level overview of the text generation process:
- Tokenization: splits input into tokens (*exact size of each token depends on model*)
- Embedding: tokens are converted into vectors (lists of numbers)
- Contextualization: context around each token is taken into account for disambiguation (*implemented with transformers based on the attention mechanism*)
- Generation: the most probable tokens to follow the end of input are generated (*like an advanced version of a smartphone's word prediction*)

Requests must include at least the following fields: (*specific to Anthropic-style API, which is implemented by some other vendors*)
1. API key (should be kept in a hidden `.env` file server-side)
2. Model name
3. Messages, in format of a list of alternating user and assistant message objects
4. Max tokens cutoff. The model does not try to reach the limit and may produce shorter messages.

The API response contains the following:
1. Message, the generated text
2. Usage, a count of input and output tokens
3. Stop reason. *Contains information on whether the request was successful.*

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
