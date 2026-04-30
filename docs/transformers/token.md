# Token

A token is the smallest possible unit an LLM uses to process and understand text. Since LLMs can only understand numbers (to perform math/computation), each unique token is assigned a specific ID (number). This allows it to start building context on each individual token and assign value to it.

## Token Limits

Token limits occur because every token requires a bit of memory and compute power and models have a maximum "context window".

## Out-Of-Vocabulary

OOV occurs when the LLM finds a token it doesn't have in its token vocab. In general, LLMs have a base set of tokens that it can use to understand text.

## Tokenization

Tokenization is the process of splitting up a sentence into multiple tokens.

**Common Types:**

`Word-Level` - Self-explanatory, split up a sentence by its words. Problem is you run into OOV often.
`Subword-Level` - Most ideal one. You split sentences based on prefix, suffix, subwords, etc. Good tradeoff for speed and OOV
`Character-Level` - Split up sentences into each individual letter. Best for avoiding OOV, but computation gets incredibly slow and expensive.
