ARG BASE_TAG=main-base
FROM winglian/axolotl-base:$BASE_TAG

ARG TORCH_CUDA_ARCH_LIST="7.0 7.5 8.0 8.6+PTX"
ARG AXOLOTL_EXTRAS=""
ARG CUDA="118"
ENV BNB_CUDA_VERSION=$CUDA

RUN apt-get update && \
    apt-get install -y vim curl

WORKDIR /workspace

RUN pip3 install --force-reinstall "peft @ git+https://github.com/huggingface/peft.git@main"
RUN git clone --depth=1 https://github.com/OpenAccess-AI-Collective/axolotl.git
# If AXOLOTL_EXTRAS is set, append it in brackets
RUN cd axolotl && \
    if [ "$AXOLOTL_EXTRAS" != "" ] ; then \
        pip install -e .[flash-attn,$AXOLOTL_EXTRAS]; \
    else \
        pip install -e .[flash-attn]; \
    fi

# fix so that git fetch/pull from remote works
RUN cd axolotl && \
    git config remote.origin.fetch "+refs/heads/*:refs/remotes/origin/*" && \
    git config --get remote.origin.fetch

# helper for huggingface-login cli
RUN git config --global credential.helper store
