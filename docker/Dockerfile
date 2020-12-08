FROM python:3.7

# fix problems with output piped through honcho
ENV PYTHONUNBUFFERED=1
ENV PYTHONIOENCODING=utf-8

RUN curl -sL https://deb.nodesource.com/setup_10.x | bash - && \
    curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
    apt-get update && \
    apt-get -y install mariadb-client nodejs wget && \
    rm -rf /var/lib/apt/lists/* && \
    npm install -g nodemon

WORKDIR /app

COPY Procfile.worker .
COPY run_worker.sh .
COPY requirements.txt .
COPY scripts/ scripts/
COPY src/ src/

RUN PIP_INSTALL="python -m pip --no-cache-dir install --upgrade" && \
    $PIP_INSTALL pip && \
    $PIP_INSTALL -r requirements.txt

ENV PYTHONPATH=src

CMD ["python", "src/app.py"]
