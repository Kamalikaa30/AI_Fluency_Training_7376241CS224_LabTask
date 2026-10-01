# Day 4 Observations

## Part A – Hand Estimates

| Model | Precision | Weights (GB) | KV (GB) | Total (GB) |
|---|---|---:|---:|---:|
| 1.5B small model | Q4_K_M | 0.855 | 0.24 | 1.2045 |
| 8B mid model | Q4_K_M | 4.56 | 1.28 | 6.424 |
| 8B mid model | FP16 | 16.00 | 1.28 | 19.008 |
| 30B large model | Q4_K_M | 17.10 | 4.80 | 24.09 |
| 70B server model | Q4_K_M | 39.90 | 11.20 | 56.21 |

## Part A Questions

### 1. Which models fit on my machine?

The program uses 8 GB as the available memory.

- 1.5B Q4_K_M: fits comfortably
- 8B Q4_K_M: fits, but tight
- 8B FP16: does NOT fit
- 30B Q4_K_M: does NOT fit
- 70B Q4_K_M: does NOT fit

### 2. How much memory does the 8B model save using Q4_K_M instead of FP16?

FP16 total = 19.008 GB

Q4_K_M total = 6.424 GB

Memory saved = 19.008 - 6.424 = **12.584 GB**

### 3. A friend has a 6 GB graphics card. What is the largest model they can run at Q4_K_M with 8K context?

The 1.5B Q4_K_M model requires about **1.20 GB**.

The 8B Q4_K_M model requires about **6.42 GB**, which is above 6 GB.

Therefore, among the models listed in Part A, the largest one that fits is **1.5B Q4_K_M**.

## Part B – Program Output

### Experiment 1: Context Length

For the 8B Q4_K_M model:

| Context | Total Memory |
|---|---:|
| 4K | 5.72 GB |
| 8K | 6.42 GB |
| 32K | 10.65 GB |
| 128K | 27.54 GB |

Observation:

The model weights remain the same, but the KV cache increases as the context length increases. Therefore, the total memory requirement increases.

### Experiment 2: Quantization

For the 8B model with 8K context:

| Precision | Total Memory |
|---|---:|
| Q3_K_M | 5.19 GB |
| Q4_K_M | 6.42 GB |
| Q5_K_M | 7.39 GB |
| Q8_0 | 10.21 GB |
| FP16 | 19.01 GB |

Observation:

Lower-bit quantization reduces memory usage, while higher precision requires more memory.

## Part C – Estimate vs Reality

Ollama was not installed on this machine, so the actual `ollama list` and `ollama ps` measurements could not be collected.

The manual states that `ollama list` provides the model size on disk and `ollama ps` shows the memory actually used while a model is running.

Therefore, no actual machine measurement is recorded here.

## Summary of Observations

### Hand estimates vs program

The hand calculations matched the Python estimator values closely.

### Effect of context length

Increasing context length increases KV-cache memory even though the model weights remain unchanged.

### Effect of quantization

Lower-bit quantization reduces memory requirements. Higher precision requires more memory.

### Important limitation

The memory values are estimates. Actual runtime memory can differ because of context length, model architecture, runtime overhead and other factors.

## Part D – Model Comparison

### Model 1 – Qwen

- Full model name and version: Qwen3-8B
- Publisher: Qwen / Alibaba Cloud
- Size available: 8B
- Size used: 8B
- Total parameters: 8.19B
- Active parameters: Not applicable for this dense 8B model
- Context window: 40K tokens on the current Ollama Qwen3 8B entry
- Licence: Apache License 2.0
- Commercial use allowed?: Yes, subject to the Apache 2.0 licence terms
- Extra conditions: Follow the Apache 2.0 licence terms
- Tool calling stated on the card?: Yes
- GGUF / Ollama build available?: Yes
- Ollama Q4_K_M size: 5.2 GB
- Memory estimate from our program: 6.42 GB for 8B Q4_K_M at 8K context
- Runs on our 8 GB machine?: Fits, but tight according to our estimate

### Model 2 – Mistral

- Full model name and version: Mistral 7B v0.3
- Publisher: Mistral AI
- Size available: 7B
- Size used: 7B
- Total parameters: 7.25B
- Active parameters: 7.25B
- Context window: 32K tokens for Mistral 7B v0.3
- Licence: Apache License 2.0
- Commercial use allowed?: Yes, subject to the licence terms
- Extra conditions: Follow the Apache 2.0 licence terms
- Tool calling stated on the card?: Yes, function calling is supported in v0.3
- GGUF / Ollama build available?: Yes
- Ollama Q4_K_M size: 4.4 GB
- Memory estimate from our program: Approximately 5.78 GB for 7.25B Q4_K_M at 8K context
- Runs on our 8 GB machine?: Fits, but relatively close to the available memory

### Model 3 – IBM Granite

- Full model name and version: Granite 4.1 8B
- Publisher: IBM
- Size available: 8B
- Size used: 8B
- Total parameters: 8.79B
- Active parameters: 8.79B
- Context window: 131K tokens
- Licence: Apache License 2.0
- Commercial use allowed?: Yes, subject to the Apache 2.0 licence terms
- Extra conditions: Follow the Apache 2.0 licence terms
- Tool calling stated on the card?: Yes
- GGUF / Ollama build available?: Yes
- Ollama Q4_K_M size: 5.3 GB
- Memory estimate from our program: Approximately 6.84 GB for 8.79B Q4_K_M at 8K context
- Runs on our 8 GB machine?: Fits, but tight according to our estimate

### Model 4 – gpt-oss

- Full model name and version: gpt-oss-20b
- Publisher: OpenAI
- Size available: 20B
- Size used: 20B
- Total parameters: 21B
- Active parameters: 3.6B per token
- Context window: 128K tokens
- Licence: Apache License 2.0
- Commercial use allowed?: Yes, subject to the Apache 2.0 licence and gpt-oss usage policy
- Extra conditions: Follow the Apache 2.0 licence and gpt-oss usage policy
- Tool calling stated on the card?: Yes
- GGUF / Ollama build available?: Yes
- Ollama model size: About 14 GB
- Quantization: MXFP4
- Memory estimate from our Q4_K_M estimator: Not directly comparable because gpt-oss uses MXFP4 rather than Q4_K_M
- Runs on our 8 GB machine?: No; the Ollama model itself is about 14 GB

## Part D – Licence Questions

### 4. Did any two sizes within the same family have different licences?

No difference was recorded in the four models selected for this comparison. The selected models use Apache License 2.0.

### 5. Which models could be used in a product you sell?

The selected models use Apache License 2.0, which permits commercial use subject to the licence terms.

### 6. Which one has conditions attached?

The models still require compliance with the Apache 2.0 licence conditions, such as retaining required copyright and licence notices.

### 7. Which cards mention tool calling or function calling?

Qwen, Mistral, IBM Granite and gpt-oss provide tool/function-calling capabilities or documentation for tool use.

### 8. Which card gives the most useful limitations information?

The model cards should be checked for their stated limitations, context limits, hardware requirements and supported use cases before deployment.

## Part E – Recommendations

### 1. 8 GB Laptop

For an 8 GB laptop, an 8B Q4_K_M model is possible but tight at an 8K context. A smaller 1.5B Q4_K_M model needs about 1.20 GB in our estimate and provides more memory headroom. The model should also support the required tool-calling workflow and have a suitable licence.

### 2. 24 GB GPU Server

A larger model can be considered on a 24 GB GPU, but context length and concurrent users must also be considered. Memory is not determined only by model weights because the KV cache and runtime overhead increase the total requirement.

### 3. Public Capstone Project

For a project published publicly on GitHub, the licence should clearly allow redistribution and the intended use. Apache 2.0 is suitable for this purpose, provided the project follows the licence requirements and any additional model-specific usage policy.

## Discussion Questions

### 1. Why can the same model stop fitting even though its weights never change?

The model weights stay the same, but the KV cache grows when the context length increases. Runtime overhead also requires memory. Therefore, the total memory requirement can increase enough that the model no longer fits.

### 2. 14B Q4 vs 8B Q8

I would test both models on the same hardware and compare memory usage, response quality, speed and the task requirements before deciding.

### 3. Why can the estimate and actual runtime memory differ?

Three possible reasons are:

1. Different context length.
2. Different model architecture and KV-cache requirements.
3. Runtime memory, activations and other overhead.

### 4. What if an "open" model forbids commercial use?

The model may still be useful for non-commercial activities if the licence permits them. For a commercial product, a model with suitable commercial-use rights should be selected.

### 5. What can be lost when using a free cloud key?

Two possible trade-offs are:

1. Dependence on an internet connection and external service.
2. Limits such as rate limits, quotas or service availability.

## Result

The memory requirements of several open models were estimated by hand and with a Python tool. The experiments showed that model memory depends not only on the number of parameters, but also on quantization and context length.

The 8B Q4_K_M model requires about 6.42 GB with an 8K context in our estimator, while the same model at FP16 requires about 19.01 GB. Increasing the context length also increases KV-cache memory.

Four model families were compared using their model information, licences, context windows, tool-calling capabilities and available model sizes.

Ollama was not installed on the machine, so the actual runtime memory comparison could not be completed. The remaining parts were completed using the Python estimator and model information.