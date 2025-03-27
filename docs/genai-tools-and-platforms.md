# GenAI Tools and Platforms

## Running LLMs locally

### [Ollama](https://github.com/ollama/ollama/)
This is the easiest way to get started. It takes care of downloading and running the model (full weight and different quantization). You can host the docker image with a CPU or GPU container on cloud if you want to share the endpoint.
One of the convenience utilities of ollama is its Modelfile. It uses dockerfile like syntax and you can specify 
- custom chat message template and fillers if you want
- custom fine tuned quantized models (as long as the [architecture](https://github.com/ollama/ollama/?tab=readme-ov-file#model-library) is supported)
[ollama/docs/modelfile.md at main · ollama/ollama](https://github.com/ollama/ollama/blob/main/docs/modelfile.md#template).

### [LlamaCpp](https://github.com/abetlen/llama-cpp-python)
LlamaCPP is another LLM execution/runtime library (kind of like huggingface transformers) but optimized to run with lower memory and on CPU. You can also target GPU for this. This is not just for Meta's llama model but a whole bunch of others. In fact Ollama uses this under the hood. I'll recommend this if you want to call a model from your own code.

Keep in mind that you cannot "reference" paths to "regular" huggingface transformer model paths in llama-cpp. It requires the model to be represented in GGUF file format. You have to download the file. Whole bunch of people uploaded tons of GGUF files for all sort of models. Here are the links to the 2 for coding. These are both `instruct` models so they won't do very well as a chat but should be able to do more complex analysis through one shot prompts.
- [Qwen/Qwen2.5-Coder-32B-Instruct-GGUF · Hugging Face](https://huggingface.co/Qwen/Qwen2.5-Coder-32B-Instruct-GGUF)
- [TheBloke/WizardCoder-Python-34B-V1.0-GGUF · Hugging Face](https://huggingface.co/TheBloke/WizardCoder-Python-34B-V1.0-GGUF)

### [Llamafile](https://github.com/Mozilla-Ocho/llamafile)
Llamafile is the stand-alone executable version of `GGUF+LlamaCpp`. Basically think of Ollama without the container. You can run it from command-line as well. But you cannot configure templates like Ollama
[Bojun-Feng/Qwen2.5-32B-Instruct-GGUF-llamafile · Hugging Face](https://huggingface.co/Bojun-Feng/Qwen2.5-32B-Instruct-GGUF-llamafile)
[Mozilla/WizardCoder-Python-34B-V1.0-llamafile · Hugging Face](https://huggingface.co/Mozilla/WizardCoder-Python-34B-V1.0-llamafile)

### Make your own GGUF
[GGUF My Repo](https://huggingface.co/spaces/ggml-org/gguf-my-repo)
If you end up fine tuning your own model or if you cannot find a GGUF for the model you want, you can use this HF space to create your own GGUF for using with `Ollama` and `llama-cpp`. You can configure your own quantization and trade-off between speed-vs-quality.
Note that - not all architecture is supported by `llama-cpp` so there are some models that won't work but most current text-generation models should be fine.

### [ONNX](https://onnxruntime.ai/docs/tutorials/)
This is another execution/runtime framework (and a file format). This is specifically designed for fast CPU inference. But you can also use GPU. It is not as memory efficient as llama-cpp but it is faster and you can do batch. Most HF repo now already comes with a subfolder called `onnx` that already comes with different quantizations. You can also use this to define your own optimization and quantization.

Another library (wrapper on top of `onnxruntime`) for the same thing is
[huggingface/optimum: 🚀 Accelerate inference and training of 🤗 Transformers, Diffusers, TIMM and Sentence Transformers with easy to use hardware optimization tools](https://github.com/huggingface/optimum)

### [Unsloth](https://unsloth.ai/)
Unsloth is really built for fine-tuning faster and with lower memory. But you can also use it for inference. Unsloth is NVIDIA GPU ONLY (and won't work with AMD). The same model runs 2-3 times faster on unsloth.
[unsloth/DeepSeek-R1-Distill-Qwen-32B-unsloth-bnb-4bit · Hugging Face](https://huggingface.co/unsloth/DeepSeek-R1-Distill-Qwen-32B-unsloth-bnb-4bit)

## Cloud Hosts/Platforms

### [Runpod.io](https://www.runpod.io/) --> so far the cheapest options for different GPUs
One of the best features of runpod.io is the Serverless deployment. You can simply choose a GPU stack and point to a huggingface repo. It will spin up an inference API endpoint for you. Best part is that you won't get charged when there is no compute load. But the cold-start time is about 6 - 10 seconds.
Runpod has a spot discount concept but you will never find anything available.

### [Digital Ocean GPU Droplet](https://www.digitalocean.com/pricing/gpu-droplets)
Cheapest H100

### Azure Spot VMs
Same set of GPUs but 85-90% cheaper. But with 80-95% availability and depends on available excess capacity in your deployment region.

### OCI
Oracle is giving out its larger hardware quite cheap. Runpod.io still beats it. But runpod charges higher for persistent storage. There are 2 options
- [Ampere A1 Compute | Oracle](https://www.oracle.com/cloud/compute/arm/) --> ARM based and you can get 56 CPUs cheaper than Azure.
- [Bare Metal Instance | Oracle](https://www.oracle.com/cloud/compute/bare-metal/) --> Running LLM on bare metal deployment is still faster. 
From my experience: Bare Metal > VM > my dead grand-daddy > Docker

## Agent and RAG Frameworks
- **Langchain**: Buggy as hell. Will NEVER recommend it.
- [Griptape](https://www.griptape.ai/): More readable code quality and less buggy. Extensive as langchain.
- [atomic-agents](https://github.com/BrainBlend-AI/atomic-agents): MUCH simpler and lightweight agent framework. But NOT as extensive as the 2 before
- [LightRAG](https://github.com/HKUDS/LightRAG): More for RAG ONLY. But it handles knowledge graph generation under the hood which makes the G part of RAG much better.

