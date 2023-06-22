FROM apache/airflow:2.6.0-python3.7
USER root
RUN echo "deb http://security.debian.org/debian-security stable-security/updates main" >> /etc/apt/sources.list

# Install gcsfuse
RUN apt-get update && \
    apt-get install --yes --no-install-recommends ca-certificates curl gnupg lsb-release && \
    echo "deb http://packages.cloud.google.com/apt gcsfuse-`lsb_release -c -s` main" > /etc/apt/sources.list.d/gcsfuse.list && \
    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add - && \
    apt-get update && apt-get install --yes --no-install-recommends gcsfuse && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

# All this for enabling installation of pillow required by pyspark
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 467B942D3A79BD29 &&\
    apt-get --allow-releaseinfo-change-suite update &&\
    apt-get -qq update && DEBIAN_FRONTEND=noninteractive apt-get -y \
    install sudo git wget  netpbm \
    python3-pyqt5 ghostscript libffi-dev libjpeg-turbo-progs \
    python3-dev cmake \
    libtiff5-dev libjpeg62-turbo-dev libopenjp2-7-dev zlib1g-dev \
    libfreetype6-dev liblcms2-dev libwebp-dev tcl8.6-dev tk8.6-dev \
    libharfbuzz-dev libfribidi-dev build-essential libssl-dev libsasl2-dev && apt-get clean

# Installing gcloud cli
RUN echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | \
    tee -a /etc/apt/sources.list.d/google-cloud-sdk.list && curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | \
    apt-key --keyring /usr/share/keyrings/cloud.google.gpg add - && \
    apt-get update -y && \
    apt-get install google-cloud-cli -y

RUN apt-get update -y && \
    apt-get install -y openjdk-11-jdk unzip && \
    apt-get autoremove -y

# Setup JAVA_HOME -- useful for docker commandline
ENV JAVA_HOME /usr/lib/jvm/java-11-openjdk-amd64
USER airflow
COPY requirements.txt ./requirements.txt
RUN pip3 install --user -r requirements.txt