# Chatbot with LLaMA-7B (QLoRA)

## Overview

This project is a chatbot demo utilizing the LLaMA-7B model fine-tuned with QLoRA (Quantized Low-Rank Adaptation). The chatbot enables efficient and lightweight deployment of a large language model while maintaining high response quality. The chatbot is implemented using **Gradio** for the user interface and **Hugging Face Transformers** for model management.

## Features

- **LLaMA-7B model**: A powerful large language model optimized for conversational AI.
- **QLoRA fine-tuning**: Reduces memory usage while preserving performance.
- **Efficient inference**: Runs on consumer-grade hardware with lower VRAM requirements.
- **Interactive chatbot**: Engages in natural language conversations with users.
- **Gradio Interface**: Provides an intuitive GUI for interaction.
- **Customizable Parameters**: Allows adjustment of temperature, top-k, top-p, and repetition penalty.
- **Advanced Features**:
  - Streaming responses for real-time interaction.
  - Memory-efficient model merging using `merge_and_unload()`.
  - Stopping criteria to avoid excessive or undesired token generation.
  - Secure logging of chat history for analysis.

## Requirements

Before running the project, ensure you have the following dependencies installed:

- Python 3.8+
- PyTorch
- Hugging Face Transformers
- Bitsandbytes (for QLoRA quantization)
- Gradio
- Requests (for optional logging)

You can install the necessary libraries using:

```bash
pip install torch transformers bitsandbytes gradio requests
```

## Model Loading & Fine-Tuning

The chatbot uses the LLaMA-7B model with adapter weights from `timdettmers/guanaco-7b`. The model is loaded into memory using `torch.bfloat16` precision for efficiency. The fine-tuning is performed using **QLoRA**, which enables efficient adaptation with reduced computational cost.

### Fine-Tuning Process:

- **Adapters from ****`timdettmers/guanaco-7b`** are integrated into the base LLaMA-7B model.
- The adapters are merged into the model using `merge_and_unload()`.
- Quantization techniques (4-bit and bfloat16) reduce memory footprint.
- The final model is optimized for inference without requiring full re-training.

## Usage

### Running the Chatbot

1. Start the chatbot using the following command:
   ```bash
   python chatbot.py
   ```
2. Open the Gradio interface in your browser and begin interacting with the chatbot.

### Gradio Interface

The chatbot runs with a **Gradio-powered UI**, allowing users to:

- **Send messages** and receive AI-generated responses in real time.
- **Adjust model parameters** dynamically:
  - `Temperature`: Controls randomness of responses.
  - `Top-k` and `Top-p`: Restrict output token sampling.
  - `Repetition penalty`: Reduces redundant word usage.
- **Clear chat history** instantly.

### Advanced UI Features

- **Dynamic streaming**: Chat responses are generated token-by-token.
- **Real-time parameter tuning**: Users can adjust response behavior on-the-fly.
- **Multi-turn conversation memory**: Maintains chat history during the session.

## Stopping Criteria

A custom stopping criteria class `StopOnTokens` is implemented to stop the model generation when specific token IDs are encountered. This prevents unnecessary token overflow and ensures concise responses.

## Conversation Logging & History Storage

The chatbot **supports conversation logging** for future analysis. If a logging server URL is provided via the `LOGGING_URL` environment variable, the chatbot logs the following data:

- **Conversation ID**: A unique identifier for each session.
- **Timestamp**: Date and time of conversation.
- **Message history**: Full conversation transcript.
- **Generation parameters**: Values of `temperature`, `top-k`, `top-p`, etc.

### Saving and Retrieving Chat History

- Conversations are logged using `requests.post(logging_url, json=data)`.
- If no logging server is available, chat history remains in session memory.
- Developers can extend history storage to databases or local files as needed.
