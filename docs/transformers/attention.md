# Attention

Attention is the concept that gives embeddings contextual meaning in sentences. At a high level overview, a token will use the surrounding tokens in the sentence to help nudge it towards a certain meaning.

## Query, Key, Value

This is how attention works mathematically. Scaled Dot-Product Attention is the most common type to ensure the scores don't get too large. We use a softmax function to make sure all weights are between 0-1

`Query (Q)` - The embedding of the current token in the sentence
`Key (K)` - The embedding of another token in the sentence
`Value (V)` - The actual weight of the key word

What happens is we multiple `Q * K^T` to find how similar the word is to each other. This helps us determine how much the key will influence the query. From there, the actual value of the key is multiplied with the weight to help create the attention which will nudge the embeddings a bit.

## Types

`Self-Attention` - A word in a sentence looks at other words in the same sentence to understand its own context
`Cross-Attention` - Typically used in translation: the model looks at the original sentence while generating the output sentence
`Multi-Head Attention` - Model uses several "heads" to look at different aspects of the text at once (ie. one head for grammar, one for meaning, etc)

## Attention Optimizations

Attention gets pretty expensive when the input is incredibly long.

### Sparse Attention

Instead of every token attending to every other token, we divide token up into local + global tokens

`Sliding Window` - This is local attention. Tokens only attend to a small fixed window of neighbors.
`Global Tokens` - Few specific tokens (ie. `[CLS]`) are allowed to attend to the entire sequence which act as a hub of information. This allows local tokens to still have some context on other tokens.

### Shared Attention Heads

An attention head is a single iteration of attention. Generally, multiple heads are used with each head focusing on a different concept (ie. grammar, semantics). Each head has its own set of Q, K, V and at the end of the MHA process all the heads are squashed together to form a coherent attention matrix. Each head has different weights (learned during training) which drastically affect what they search for.

This technique approximates the MHA process by reducing the number of Keys and Value stored in memory, which helps speed up inference and reduce KV cache size.

`Multi-Query Attention (MQA)` - All attention heads share a single Key and Value head. Queries remain multi-headed. Incredibly fast, but can bottleneck its compute power.
`Grouped-Query Attention (GPA)` - A middle ground where Queries are divided into groups and each group shares one Key and Value head.

### KV Cache

Key Value (KV) Caching revolves around the idea that new tokens in inputs only need to interact with all previous tokens. Therefore, we can save on compute by just keeping the previous keys and values embeddings in a cache.

**Steps:**

`Prefill` - When you send your initial prompt, the model calculates the K and V vectors for all your words and stores them
`Decoding` - When generating the next word, the model only calculates the Q, K, and V for that single new token
`Lookup` - The LLM pulls from the KV cache to perform attention
`Update` - The new token's K and V is added to the cache

> While this is great, the tradeoff is it consumes a lot of VRAM. In long conversations, the cache can become so large that it exceeds the GPU's memory.

## Paged Attention

This is a memory optimization technique inspired by VRAM and Paging in traditional operating systems. Designed to solve the inefficient storage of KV Cache.

PagedAttention breaks the KV cache into small, fixed-size blocks. This allows the blocks to be able to stored independently. The model will see the KV cache as a continuous sequence of tokens, but the physical blocks are scattered across the GPU's memory. The Block Table (mapping system) keeps track of which tokens are stored in which blocks.

## Latent Attention

Used to compress the KV Cache. In standard attention, the KV cache grows linearly with context length, but in latent attention the model compresses the KV into a "latent vector". This latent vector is then stored and uncompressed during inference.
