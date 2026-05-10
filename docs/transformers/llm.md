# LLM

A large language model (LLM) is a machine learning model that assigns probabilities to sequences of tokens.

## Mixture of Experts (MoE)

A neural network architectural design that allows a model to have a massive number of parameters while keeping the computational cost relatively low during inference. Instead of being one giant, monolithic block where every part of the brain "fires" for every word, an MoE model acts more like a specialized panel of experts

An MoE model replaces the standard FFNN layer and replaces it with an MoE layer.

### Components

`The Experts` - These are smaller, independent neural networks ("experts") housed within the larger model. A model will have multiple of these experts
`THe Gating Network (Router)` - This is the "manager". When a token comes in, the router decides which experts are best suited to handle it. 1-2 experts are typically selected per token

### Pros/Cons

**Advantages:**

`Efficiency` - You can get the compute of a massive model with the speed and energy consumption of a smaller one
`Faster Training` - Since only a fraction of the model is used per step, it can often be trained to a higher level of performance in a shorter amount of time
`Specialization` - Different experts can be used to handle different types of data (ie. one's good at Python, another is good at excel)

**Disadvantages:**

`VRAM Requirements` - These models are typically huge, requiring lots of GPU
`Routing Complexity` - Routing can be pretty difficult. If the router isn't well trained, the entire system falls apart
`Fine-tuning Difficulty` - Since the logic is so much more advanced than a standard LLM, it'll be harder to optimize

## Guided Decoding

Guided decoding helps ensure the LLM generates outputs in a specific format (ie. JSON). This is done mathematically to restrict the model's choices at every single step. While in standard decoding the model can pick any token, in guided decoding a formal `grammar engine` sits between the model and the output
