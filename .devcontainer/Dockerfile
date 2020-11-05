ARG PYTHON_VERSION=3.9.0
FROM python:${PYTHON_VERSION}-slim-buster

# Install wkhtmltopdf w/ patched qt
# See: https://wkhtmltopdf.org/downloads.html#why-do-you-have-static-builds-with-patched-qt
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        fontconfig \
        libfreetype6 \
        libjpeg62-turbo \
        libpng16-16 \
        libx11-6 \
        libxcb1 \
        libxext6 \
        libxrender1 \
        xfonts-75dpi \
        xfonts-base \
        curl && \
    curl -sSL https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox_0.12.6-1.buster_amd64.deb -o wkhtmltox.deb && \
    dpkg -i wkhtmltox.deb && \
    rm wkhtmltox.deb

# Install Chrome and chromedriver
RUN apt-get install -y --no-install-recommends \
        unzip \
        gnupg2 && \
    curl -sSL -o - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add && \
    echo "deb [arch=amd64]  http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list && \
    apt-get update && \
    apt-get install -y --no-install-recommends google-chrome-stable && \
    CHROME_DRIVER_VERSION=`curl -sSL https://chromedriver.storage.googleapis.com/LATEST_RELEASE` && \
    curl -sSL -o chromedriver.zip https://chromedriver.storage.googleapis.com/$CHROME_DRIVER_VERSION/chromedriver_linux64.zip && \
    unzip chromedriver.zip && \
    rm chromedriver.zip && \
    mv -f chromedriver /usr/local/bin/chromedriver && \
    chown root:root /usr/local/bin/chromedriver && \
    chmod 0755 /usr/local/bin/chromedriver

# Install poetry
ENV POETRY_HOME=/etc/poetry
ENV PATH=$POETRY_HOME/bin:$PATH
RUN curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python -

# Install other tools
RUN apt-get install -y --no-install-recommends \
        exa \
        git && \
    curl -fsSL https://starship.rs/install.sh | bash -s -- --yes && \
    curl -sSL -o bat.deb https://github.com/sharkdp/bat/releases/download/v0.16.0/bat_0.16.0_amd64.deb && \
    dpkg -i bat.deb && \
    rm bat.deb

COPY .devcontainer/.bashrc /root/.bashrc
