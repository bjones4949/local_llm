FROM python:3.12-slim

RUN apt-get update && apt-get install -y --no-install-recommends \
      libtcmalloc-minimal4 libnuma1 \
    && rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir vllm-cpu==0.30.0 \
      --index-url https://download.pytorch.org/whl/cpu \
      --extra-index-url https://pypi.org/simple

ENV LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libtcmalloc_minimal.so.4

ENTRYPOINT ["vllm", "serve"]