FROM ruby:2.7.1

# throw errors if Gemfile has been modified since Gemfile.lock
# RUN bundle config --global frozen 1

RUN mkdir -p /usr/src/osb

RUN apt-get update && apt-get install -y \
    curl \
    sudo \
    default-libmysqlclient-dev \
    #npm --no-install-recommends \
    netcat

RUN curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash - && sudo apt-get install -y nodejs --no-install-recommends
#RUN apt-get update && apt-get install -y curl sudo default-libmysqlclient-dev && curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash - && sudo apt-get install -y nodejs

RUN rm -rf /var/lib/apt/lists/*
#RUN apt-get update && apt-get install -y nodejs npm --no-install-recommends && rm -rf /var/lib/apt/lists/*
#RUN apt-get -q update && apt-get -qy install netcat

RUN gem install bundler -v 2.4.22 --no-document

WORKDIR /usr/src/osb


COPY Gemfile* /usr/src/osb/
COPY Gemfile Gemfile.lock ./
COPY vendor/engines ./vendor/engines/

RUN bundle install
COPY . /usr/src/osb
RUN npm install -g npm@8.19.4 && sudo npm install --global yarn@1.22.4 && yarn install

COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]

COPY ./docker/web/wait-for-it.sh ./docker/web/wait-for-it.sh
RUN chmod +x ./docker/web/wait-for-it.sh

EXPOSE 3000
CMD ["rails", "server", "-b", "0.0.0.0"]
