# syntax=docker/dockerfile:experimental
FROM pytorch/pytorch:1.5-cuda10.1-cudnn7-runtime

RUN apt-get update --allow-unauthenticated && \
    apt-get install -y gnupg2 \
            curl \
            wget && \
    apt-key del 7fa2af80  && \
    rm -f /etc/apt/sources.list.d/cuda*.list && \
    curl https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-keyring_1.0-1_all.deb \
    -o cuda-keyring_1.0-1_all.deb && \
    dpkg -i cuda-keyring_1.0-1_all.deb

WORKDIR /src

ARG PRETRAINED_MODEL_NAME_OR_PATH
ENV PRETRAINED_MODEL_NAME_OR_PATH ${PRETRAINED_MODEL_NAME_OR_PATH}
ARG SERVICE_PORT
ENV SERVICE_PORT ${SERVICE_PORT}
ARG FINETUNED_GPT_URL
ENV FINETUNED_GPT_URL ${FINETUNED_GPT_URL}

COPY ./requirements.txt /src/requirements.txt
RUN pip install -r /src/requirements.txt

WORKDIR /data
RUN wget ${FINETUNED_GPT_URL} -q -O ./filtered_ROCStories_gpt_medium.pt

WORKDIR /src

COPY . /src

CMD gunicorn --workers=1 server:app -b 0.0.0.0:${SERVICE_PORT} --timeout=1200
