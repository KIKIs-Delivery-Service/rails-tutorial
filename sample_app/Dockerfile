FROM ubuntu:20.04

ARG app_name='sample_app'

# Setup directorys
RUN mkdir /rails-tutorial &&\
    mkdir /rails-tutorial/environment
WORKDIR /rails-tutorial/environment

# Install packages
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update -y && apt-get install -y \
    git git-secrets curl tzdata
# Git prompt
RUN curl https://raw.githubusercontent.com/git/git/master/contrib/completion/git-prompt.sh
# Node js
RUN curl -fsSL https://deb.nodesource.com/setup_16.x | bash - &&\
    apt-get install -y nodejs
## Yarn
RUN corepack enable npm pnpm yarn && \
    corepack prepare yarn@1.22.19 --activate
## Postgres
RUN apt-get install -y \
    postgresql postgresql-contrib libpq-dev
## Heroku
RUN curl https://cli-assets.heroku.com/install-ubuntu.sh | sh

# Prefferences
ENV TZ Asia/Tokyo
RUN { \
    echo '# Customize ls command'; \
    echo 'alias ls="ls --color=auto"'; \
    echo 'alias ls="ls -l"'; \
    echo '# Enable git completion'; \
    echo 'source /usr/share/bash-completion/completions/git'; \
    echo '# Display git branch on prompt'; \
    echo 'source /etc/bash_completion.d/git-prompt'; \
    echo 'GIT_PS1_SHOWDIRTYSTATE=true'; \
    echo 'GIT_PS1_SHOWUNTRACKEDFILES=true'; \
    echo 'GIT_PS1_SHOWSTASHSTATE=true'; \
    echo 'GIT_PS1_SHOWUPSTREAM=auto'; \
    echo "export PS1='\\[\\033[32m\\]\\u@docker\\[\\033[00m\\] \\[\\033[33m\\]\\W\\[\\033[1;36m\\]\$(__git_ps1 \" (%s)\")\\[\\033[00m\\] \\\$ '"; \
    } >> ~/.bashrc

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
RUN rails _6.0.4_ new ${app_name}

# Setup application
WORKDIR /rails-tutorial/environment/${app_name}
COPY Gemfile /rails-tutorial/environment/${app_name}/Gemfile
COPY package.json /rails-tutorial/environment/${app_name}/package.json
RUN bundle _2.2.17_ config set --local without 'production' && \
    bundle update && \
    bundle _2.2.17_ install --path vendor/bundle
RUN yarn install && \
    yarn add jquery turbolinks @rails/ujs @rails/activestorage chanel
RUN bundle update && \
    rails webpacker:install &&\
    rails webpacker:compile
# Prevent error when you run 'bundle exec guard'
ENV LANG C.UTF-8


# Run application
# CMD [ "rails", "server", "-b", "0.0.0.0" ]
