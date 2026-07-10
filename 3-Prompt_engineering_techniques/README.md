# Prompt engineering techniques

## Prompt engineering overview

Covers [Prompt engineering](https://anthropic.skilljar.com/claude-with-the-anthropic-api/287745).

Prompt engineering can be done through an iterative improvement process:
1. Set a goal, clearly defining what the prompt should accomplish
2. Write an initial prompt to serve as a baseline
3. Evaluate the prompt
4. Improve performance through prompt engineering techniques
5. Re-evaluate the new prompt to see whether and by how much metrics improve

Anthropic provides a `PromptEvaluator` class for dataset generation and model grading.
Using a small number (2-3) test cases during development and increasing for final validation can help speed up development *and save on token usage*.

## Being clear, direct, and specific

Covers [Being clear and direct](https://anthropic.skilljar.com/claude-with-the-anthropic-api/287744) and [Being specific](https://anthropic.skilljar.com/claude-with-the-anthropic-api/287740).

*Part of optimizing prompts can be done just by changing writing style and being aware of good practices*.

Clear communication
- Use simple language that is easily understood
- Start with a straightforward statement of the task instead of being vague or having a long prelude

Direct instructions
- Use instructions, not questions
- Start instructions with direct action verbs like "Write," "Create," or "Generate"

Prompts should include specific guidelines to ensure that generated responses fulfill all requirements.
There are two main types of guidelines:
1. A list of qualities the output should have, including both quantitative metrics like output length and qualitative metrics like tone and style.
2. A list of process steps to follow before arriving at a final answer. *This technique is generally known as Chain-of-Thought prompting, and it* [has been shown](https://dl.acm.org/doi/10.5555/3600270.3602070) *to improve performance on reasoning tasks even after adjusting for token use*.

Quality guidelines should be used in almost all prompts.
Process steps should be used for tasks that require reasoning or consideration of multiple potential perspectives.

## XML tags and examples

Covers [Structure with XML tags](https://anthropic.skilljar.com/claude-with-the-anthropic-api/287741) and [Providing examples](https://anthropic.skilljar.com/claude-with-the-anthropic-api/287746).

*XML tags and few-shot prompting are similar to Markdown in being specific methods of formatting that are generally useful*.

XML tags improve prompts by making them more easily parseable for the model.
For example, an eval prompt may include `<task_description>`, `<task_inputs>`, and `<criteria>` clearly marked with XML tags.
The benefit comes not in following any pre-defined XML schema, but in clearly marking the purpose of each section.
A specific tag name like `<sales_records>` is better than a generic one like `<data>`.

Another structure to use in prompts is one or a few examples included in the prompt.
Example input can be incuded in a tag like `<sample_input>` with the desired response in `<ideal_output>`.
Showing what you want instead of just describing it helps to reduce ambiguity and lead to relevant responses.
Including edgecases also helps to ensure that the response is appropriate for specific potentially problematic situations.
*Including examples in an LLM prompt is known as multi-shot prompting or in-context learning*.
*It is technically different but conceptually similar to few-shot learning, in which a model trained on a general problem is given a small number of data points for a new class of problem*.
*The concept is that an LLM is already trained on the general problem of text generation, and including examples helps to specialize the text generation for the task at hand*.
*Including examples in the prompt adds more tokens, but comes at a much lower upfront cost than fine-tuning the model through the traditional method of training to update parameters*.
