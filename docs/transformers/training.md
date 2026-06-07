# Training

## Training Steps

`Data Preparation` - This is where you prepare the dataset needed for the model to train on
`Initialization` - This is where you define the model's structural blueprint (ie. parameters) and how it will process information
`Training` - Consists of three phases:

1. `Forward Pass` - This is when we feed input into the model to compute its loss
2. `Backwards Pass` - This is where back-propagation helps us compute the gradients (needed for weight update)
3. `Weight Update` - This is where the optimizer takes those computed gradients and helps the model updates its weights

## Training Types

### Pretraining

This is where a model is trained on large amounts of text to get a generic high-level NLP. This allows an LLM to understand language.

### Finetuning

Instead of training a model a model from scratch, we can take a pretrained model and finetune it for our use-case.

#### LoRA

A major problem of finetuning is that it's resource intensive and requires a lot of GPUs. Low-Rank Adaptation (LoRA) solves this by freezing the weights of the pretrained model and then adding a smaller layer ontop of it that it uses to finetune the model. This will help nudge the model towards the designed outcome.

**Drawbacks:**
LoRA is only as effective as the base model is. Since the added matrix layer is so small, you can't really do many complex tasks. It is really used to teach a model how to act (ie. instruction training, formatting). If you need to teach models new facts, finetuning is necessary.

It also struggles when the batch size increases. Since the matrix is so small, huge amounts of data can muddle the weights

> Since you're only training a slim additional matrix, you can easily swap it out for different tasks.

### QLoRA

A technique that combines model quantization and LoRA to massively save on memory. Regular LoRA freezes base model weights at FP16 (16 bytes), but QLoRA compresses these frozen weights down to NF4 (4 bytes).

### Preference Tuning

Helps teach a model what humans actually prefer. It's also referred to as alignment which ensures that the model is safe and doesn't give dangerous advice.

## Chinchilla Law

A law that focuses on data efficiency of model. It states that you need roughly 20 tokens of data for every 1 parameter in your model for optimal efficiency.

## Knowledge Cutoff

AI only has as much knowledge as it was last trained on. This makes it hard to learn new data after a while.

## Parallelism

When training LLMs, a single GPU often runs out of memory or takes too long to process the data. To solve this, we distribute the workload across multiple GPUs.

### Data Parallelism

Idea is to divide batches of data across devices. Model is also replicated on each device. This allows for parallelized training. The only problem is wasted space as the model's parameters, gradients, and optimizer must be cloned.

#### ZeRO

Stands for Zero Redundancy Optimization. Idea is to optimize redundant information across devices by sharding them.

`ZeRO-1` - Every cloned model will share an optimizer
`ZeRO-2` - Every cloned model will share optimizer + gradients
`ZeRO-3` - Every cloned model will share optimizer + gradients + parameters

### Model Parallelism

We do this when the model is too big to fit into the memory of a single GPU. Therefore, we split the model up and a single piece of data needs to travel across multiple GPUs to finish a single pass.

## Mixed Precision Training

Speeds up model training and slashes VRAM usage by using FP16 and FP32 bit floating points at the same time. In the past, models were trained entirely on FP32. Mixed precision abuses the fact that models can still do calculations with lower precision formats without hurting the accuracy of the final model.

## Preference Tuning

Adjusting an LLM so it becomes aligned with human morals, values, and safety.

### Dataset

Preference training requires a high quality dataset to allow the model be trained to make good preferences.

**Types:**

- `Pointwise` - Every sentence/data point is mapped to a singular numerical value. This determines how positive or negative a sentence is
- `Pairwise` - Every sentence/data point is compared to another sentence or data point. This helps the LLM distinguish which sentences are better than other ones. This one is the preferred method as its incredibly simple and easy to generate the data
- `Listwise` - A list of sentences/data points are ranked against each other and ordered from best to worst.

### RLHF

Reinforcement Learning from Human Feedback consists of:

1. Gather Comparison Data - Review output for a single prompt and rank them from best to worst
2. Train a Reward Model - A smaller model is trained on this ranking data to mimic human judgement assigning weights to any text (ie. +3 for a good answer, -3 for a bad one)
3. Optimize via Reinforcement Learning - Now the main LLM generates responses and the Reward model scores them. A Reinforcement Learning algorithm (ie. PPO) updates the LLMs weights

> RLHF has a big drawback of memory consumption due to having to load in 4 huge models in memory at once during training (policy model, ref model, value model, reward model)

#### Bradley-Terry Formula

This is the mathematical formula used to train the reward model in RHLF. This is used in pairwise data and helps assign a probability that a human would choose response A over response B. It helps numerically quantify this choice so the LLM can understand.

#### PPO

This is a optimization algorithm that adjusts the weights of the LLM based on the results from the reward model. An important concept to understand here is it'll take the adjusted reward, but also factor in weights from the base model (pretrained model not finetuned one) to make sure the new adjusted weights don't deviate too much from the original model.

#### BoN

This is a technique where we don't adjust the weights of the finetuned model. Instead, we simply have the finetuned model generate a bunch of responses (with varying temperatures) and have the reward model pick the best one.

> Note: This is a suboptimal technique due to the heavy a compute costs.

### DPO

Direct Preference Optimization solves the issue of RLHF being unstable and expensive due to the fact that multiple models are kept in memory. DPO treats preference tuning as simple binary classification - increasing the probability of generating the chosen response and decreasing the probability of generating the rejected response. It does this by adjusting the loss function in a supervised way so the LLM adjusts its weights based on the preference data as well.

> DPO achieves similar or better results as RLHF
