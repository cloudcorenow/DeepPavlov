FROM deeppavlov/base-gpu:0.12.1

ARG CONFIG
ARG COMMIT
ARG SERVICE_PORT
ARG SRC_DIR

ENV CONFIG=$CONFIG
ENV SERVICE_PORT=$SERVICE_PORT

COPY ./annotators/entity_linking_deepy/requirements.txt /src/requirements.txt
RUN pip install --upgrade pip && pip install -r /src/requirements.txt

RUN cd DeepPavlov && \
    git config --global user.email "you@example.com" && \
    git config --global user.name "Your Name" && \
    git fetch --all --tags --prune && \
    git checkout 0.13.0 && \
    pip install -e .

COPY $SRC_DIR /src

WORKDIR /src

CMD python -m deeppavlov riseapi $CONFIG -p $SERVICE_PORT -d
