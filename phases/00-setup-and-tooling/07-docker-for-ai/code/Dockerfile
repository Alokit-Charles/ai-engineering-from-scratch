FROM nvidia/cuda:12.4.1-devel-ubuntu22.04
ENV DEBIAN_FRONTEND=noninteractive
ENV PYTHONUNBUFFERED=1

# ca-certificates is required for the curl | sh install to work over https
RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    curl \
    ca-certificates \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# Install uv via the official installer script (same as your local setup)
RUN curl -LsSf https://astral.sh/uv/install.sh | sh
ENV PATH="/root/.local/bin:$PATH"

# Install Python 3.12 via uv — deterministic, no distro version drift
ENV UV_PYTHON_INSTALL_DIR=/opt/uv/python
RUN uv python install 3.12
ENV UV_PYTHON=3.12

WORKDIR /workspace

RUN uv venv /opt/venv --python 3.12
ENV VIRTUAL_ENV=/opt/venv
ENV PATH="/opt/venv/bin:$PATH"

RUN uv pip install --no-cache \
    torch==2.4.1 \
    torchvision==0.19.1 \
    torchaudio==2.4.1 \
    --index-url https://download.pytorch.org/whl/cu124

RUN uv pip install --no-cache \
    numpy \
    pandas \
    scikit-learn \
    matplotlib \
    jupyter \
    transformers \
    datasets \
    accelerate \
    safetensors \
	qdrant-client==1.12.* \
	flask

VOLUME ["/workspace", "/models"]
EXPOSE 8888
CMD ["python"]