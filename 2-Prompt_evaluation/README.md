# Prompt evaluation

## Prompt evaluation overview

Covers [Prompt evaluation](https://anthropic.skilljar.com/claude-with-the-anthropic-api/287731) and [A typical eval workflow](https://anthropic.skilljar.com/claude-with-the-anthropic-api/287736).

Prompt evaluation refers to the approach of systematically measuring prompt effectiveness.
Basic manual tests are convenient, but often result in prompts that fail when deployed to real users.
Creating a prompt evaluation pipeline has upfront costs, but improves reliability.

A data-driven prompt evaluation approach enables the following benefits:
- Identifying weaknesses in testing rather than production
- Objective comparisons between candidate prompts
- Measurable improvement across iterations of the prompt
- Reliable applications

Key steps of a tpyical eval workflow:
1. Draft a system prompt to serve as the baseline
2. Create an eval database of sample user questions to test
3. Generate model responses for each question given the system prompt
4. Evaluate quality of generated results as a quantitative value, usually as a score from 1 to 10.
5. Test out different system prompts using the same user questions and compare quality against the baseline

## Generating test datasets

Covers [Generating test datasets](https://anthropic.skilljar.com/claude-with-the-anthropic-api/287739).

The user messages that potential system prompts are tested on can be generated mostly automatically.
An LLM can be used to generate structured data containing sample messages fulfilling some desired constraints.
The structured data can then be processed similarly to unit tests for classical programming tasks.

## Grading 

Covers [Running the eval](https://anthropic.skilljar.com/claude-with-the-anthropic-api/287743), [Model based grading](https://anthropic.skilljar.com/claude-with-the-anthropic-api/287742), and [Code based grading](https://anthropic.skilljar.com/claude-with-the-anthropic-api/287737).

The grading procedure should, for each piece of test data, output the generated response and a score for the generated response.
There are three main approaches to assigning scores to prompts, each with different strengths.

- Code graders include programmatic checks on generated content. They are especially suited to checking for format and syntactic validity, but can also involve heuristic measurements of subjective qualities like readability.
- Model graders take the generated responses and grade them by using another LLM call. They are able to quickly evaluate flexible criteria and are suited to score the level of task-following from a model.
- Human graders are the most flexible option, but have high cost for evaluating large amounts of data.

## WIP

[Exercise on prompt evals](https://anthropic.skilljar.com/claude-with-the-anthropic-api/287738)
[Code based grading](https://anthropic.skilljar.com/claude-with-the-anthropic-api/287737)
