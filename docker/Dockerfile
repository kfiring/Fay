FROM ubuntu:24.04

# disable interactive prompts during package installation
ARG DEBIAN_FRONTEND=noninteractive

# set timezone to Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo "Asia/Shanghai" > /etc/timezone

# change apt source to aliyun (compatible with Ubuntu 24.04)
RUN find /etc/apt/sources.list* -type f -exec sed -i 's/archive.ubuntu.com/mirrors.aliyun.com/g' {} \;

# install essential packages
RUN apt-get update && \
    apt-get install -y build-essential curl wget git vim && \
    apt-get clean

# install python3.12
RUN apt-get update && \
    apt-get install -y python3.12 python3.12-venv python3.12-dev python3-full && \
    apt-get clean

# set python3.12 as default python3 and python
RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.12 1 && \
    update-alternatives --install /usr/bin/python python /usr/bin/python3.12 1

# create virtual environment and configure pip inside it(we have to use virtual environment or it will complain about 'error: externally-managed-environment ...')
RUN python3 -m venv /opt/venv && \
    /opt/venv/bin/pip install --upgrade pip && \
    /opt/venv/bin/pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/ && \
    /opt/venv/bin/pip config set global.trusted-host mirrors.aliyun.com

ENV PATH="/opt/venv/bin:$PATH"

# copy codes to /fay
WORKDIR /fay
COPY ./* /fay/

# install dependencies inside virtual environment
RUN pip install --no-cache-dir -r requirements.txt

# set entrypoint
ENTRYPOINT ["python", "main.py", "start"]