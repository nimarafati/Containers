# Copied from https://github.com/FredHutch/docker-samtools-bwa/blob/master/Dockerfile

FROM ubuntu:18.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update -qq \
  && apt-get install -qq bzip2 gcc g++ make zlib1g-dev wget libncurses5-dev liblzma-dev libbz2-dev pigz libcurl4-openssl-dev \
  && apt-get install -y python3-pip python3-dev python3.8 awscli jq \
  && cd /usr/local/bin \
  && ln -s /usr/bin/python3.8 python \
  && pip3 --no-cache-dir install --upgrade pip \
  && rm -rf /var/lib/apt/lists/* \
  && python3 -m pip install --upgrade awscli

RUN python3 -m pip install numpy boto3 pandas pysam
RUN python --version

ENV BWA_VERSION 0.7.17
ENV SAMTOOLS_VERSION 1.12

RUN cd /opt/ \
    && wget https://github.com/lh3/bwa/releases/download/v${BWA_VERSION}/bwa-${BWA_VERSION}.tar.bz2 \
    && tar -xjf bwa-${BWA_VERSION}.tar.bz2 \
    && rm -f bwa-${BWA_VERSION}.tar.bz2 \
    && cd /opt/bwa-${BWA_VERSION}/ \
    && make

RUN cd /opt/ \
    && wget https://github.com/samtools/samtools/releases/download/${SAMTOOLS_VERSION}/samtools-${SAMTOOLS_VERSION}.tar.bz2 \
    && tar -xjf samtools-${SAMTOOLS_VERSION}.tar.bz2 \
    && rm -rf samtools-${SAMTOOLS_VERSION}.tar.bz2  \
    && cd samtools-${SAMTOOLS_VERSION}/ \
    && make && make install

ENV PATH="/opt/bwa-${BWA_VERSION}/:/opt/samtools-${SAMTOOLS_VERSION}/:${PATH}"