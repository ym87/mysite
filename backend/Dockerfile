FROM python:3.8-slim

ENV PYTHONUNBUFFERED 1

RUN groupadd user && useradd --create-home --home-dir /home/user -g user user
WORKDIR /home/user/app/backend

# Install system dependencies
RUN apt-get update && apt-get install gcc build-essential libpq-dev -y && \
    python3 -m pip install --no-cache-dir pip-tools

# install python dependencies
ADD *requirements.in /home/user/app/backend/
RUN pip-compile requirements.in > requirements.txt && \
    pip-compile dev-requirements.in > dev-requirements.txt

RUN pip install -r requirements.txt && \
    pip install -r dev-requirements.txt

# Clean the house
RUN apt-get purge libpq-dev -y && apt-get autoremove -y && \
    rm /var/lib/apt/lists/* rm -rf /var/cache/apt/*


################
################
#追加
RUN pip install --upgrade pip && \
    pip install jupyter

RUN mkdir workbook \
          ~/.jupyter && \
    touch ~/.jupyter/jupyter_notebook_config.py

RUN apt-get update \
    && apt-get install -y git \
    && apt-get install -y apt-utils \
    && apt-get install -y mecab \
    && apt-get install -y libmecab-dev \
    && apt-get install -y mecab-ipadic-utf8\
    && apt-get install -y make\
    && apt-get install -y curl\
    && apt-get install -y xz-utils\
    && apt-get install -y file\
    && apt-get install -y sudo\
    && apt-get install -y wget

RUN git clone --depth 1 https://github.com/neologd/mecab-ipadic-neologd.git\
    && cd mecab-ipadic-neologd\
    && bin/install-mecab-ipadic-neologd -n -y

RUN apt-get install -y libpq-dev

#vscodeデバッグエラー対策
RUN sudo apt-get -y install locales dialog \
    && localedef -f UTF-8 -i ja_JP ja_JP
    
###########

ADD backend/ /home/user/app/backend

USER user
CMD gunicorn mysite.wsgi --log-file - -b 0.0.0.0:8000 --reload


