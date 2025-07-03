https://hamel.dev/blog/posts/evals-faq/

AI Evals at Pycon this year
# Evaluation Metrics & Methodology

* What metrics do you use (e.g., BERTScore, ROUGE, F1)? Are similarity metrics still useful?

* Do you use step-by-step evaluations or evaluate full responses?

* How do you evaluate VLM (vision-language model) summarization? Do you sample outputs or extract named entities?

* How do you approach offline (ground truth) vs. online evaluation?

* How do you handle uncertainty or "don’t know" cases? (Temperature settings?)

* How do you evaluate multi-turn conversations?

* A/B comparisons and discrete labels (e.g., good/bad) are easier to interpret.

* It’s important to counteract bias toward your own favorite eval questions—ensure a diverse dataset.

## Prompting & Models

* Do you modify prompts based on the specific app being evaluated?

* Where do you store prompts—text files, Prompty, database, or in code?

* Do you have domain experts edit or review prompts?

* How do you choose which model to use?

## Evaluation Infrastructure

* How do you choose an evaluation framework?

* What platforms do you use to gather domain expert feedback or labels?

* Do domain experts label outputs or also help with prompt design?

## User Feedback & Observability

* Do you collect thumbs up / thumbs down feedback?

* How does observability help identify failure modes?

* Do models tend to favor their own outputs? (There's research on this.)

I personally work on adding evaluation to our most popular Azure RAG samples, and put a Textual CLI interface in this repo that I've found helpful for reviewing the eval results: https://github.com/Azure-Samples/ai-rag-chat-evaluator

