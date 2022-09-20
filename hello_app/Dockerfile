FROM ubuntu:18.04

# Add non-root user
# RUN useradd -D railser && chown -R railser /rails-tutorial

# Prefference
ENV TZ Asia/Tokyo
RUN echo 'export PS1="\u@docker \W 🐳 "' >> /root/.bashrc

# Setup directorys
RUN mkdir /rails-tutorial &&\
    mkdir /rails-tutorial/environment
WORKDIR /rails-tutorial/environment

# Install packages
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update -y && apt-get install -y \
    git curl tzdata
RUN curl -fsSL https://deb.nodesource.com/setup_16.x | bash - &&\
    apt-get install -y nodejs
## Yarn
RUN corepack enable npm pnpm yarn
RUN corepack prepare yarn@3.2.1 --activate

# Install rvm
RUN curl -sSL https://get.rvm.io | bash
ENV PATH $PATH:/usr/local/rvm/bin

# Install ruby
RUN /bin/bash -l -c "rvm install 2.7.6 && rvm --default use 2.7.6"
ENV PATH $PATH:/usr/local/rvm/rubies/ruby-2.7.6/bin

# Setup gem
ENV GEM_HOME /usr/local/rvm/rubies/ruby-2.7.6/lib/ruby/gems/2.7.0
ENV GEM_PATH /usr/local/rvm/rubies/ruby-2.7.6/lib/ruby/gems/2.7.0
RUN echo "gem: --no-document" >> ~/.gemrc

# Install rails
RUN /bin/bash -l -c "gem install rails -v 6.0.4"

# Install bundler
RUN /bin/bash -l -c "gem install bundler -v 2.2.17 --force"

# Create application
RUN rails _6.0.4_ new hello_app

# Setup application
WORKDIR /rails-tutorial/environment/hello_app
COPY Gemfile /rails-tutorial/environment/hello_app/Gemfile
COPY package.json /rails-tutorial/environment/hello_app/package.json
RUN bundle update && bundle _2.2.17_ install
RUN yarn install
RUN sed -i -e 's/check_yarn_integrity: true/check_yarn_integrity: false/g' config/webpacker.yml
RUN rails webpacker:install
RUN sed -i -e 's/check_yarn_integrity: true/check_yarn_integrity: false/g' config/webpacker.yml

# Run application
# CMD [ "rails", "server", "-b", "0.0.0.0" ]