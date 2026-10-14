# Local LLM evaluation

Start an Ollama server and install a model you can run locally. This repository does not automatically download one and requires no paid API. Then:

```bash
python run_ollama.py examples/input.json --model YOUR_INSTALLED_MODEL
python tool.py results/ollama/input.json --output results/ollama/rechecked.json
```

Only `http://localhost`, `http://127.0.0.1` and `http://[::1]` endpoints are accepted by the CLI. Case gold labels and scope labels are withheld from the model prompt. Each response is checkpointed to `raw.json`; final reports retain the prompt, model name, elapsed time and decoding settings. Preserve the server response's model metadata and digest when designing a reproducible study.

The adapter uses [Ollama's documented chat API](https://docs.ollama.com/api/chat), JSON output, temperature 0 and seed 0. Invalid JSON/schema becomes abstention. Network/server errors stop the run rather than generating fake responses. A real quote can accompany a wrong scope classification, so the quote guard is assessed separately from the human gold labels.

**Validation status:** mocked HTTP/response tests verify payload construction, gold-label exclusion and malformed-response behavior. No actual local LLM inference run was performed for the initial release because this machine did not have an Ollama server installed. The bundled predictions are hand-authored synthetic fixtures, not LLM results. Run on the fixture to check integration, then build a human-adjudicated corpus before comparing models.
