FROM arm64v8/python:3.7.9

RUN mkdir /app \
&& apt-get update -y \
&& apt-get install -y \
    build-essential \
    libssl-dev libffi-dev \
    python3-dev python3-pip \
    libsasl2-dev libldap2-dev \
&& rm -rf /var/lib/apt/lists/*            

RUN pip install apache-superset

ENTRYPOINT ["superset"]