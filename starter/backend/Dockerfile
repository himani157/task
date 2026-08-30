FROM public.ecr.aws/docker/library/python:3.10-slim

ENV FLASK_RUN_HOST=0.0.0.0

RUN useradd -u 1000 -m app

RUN apt-get update && \
    apt-get install -y \
    pcre2-utils \
    libpcre3-dev \
    build-essential \
    gcc \
    libssl-dev \
    libffi-dev \
    && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /var/www/app && mkdir /app

RUN pip install --upgrade pip setuptools wheel

# Install uwsgi first with specific options
RUN pip install --no-cache-dir uwsgi

# Then install the rest
COPY requirements.txt /app/
RUN pip install --no-cache-dir -r /app/requirements.txt

COPY . /app

CMD uwsgi --http :5000 --master --enable-threads -s /var/www/app/app.sock --manage-script-name --mount /=app:app