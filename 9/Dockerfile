FROM jruby:9.2.9.0

RUN apt-get update -qq && apt-get dist-upgrade -y

RUN curl -sL https://deb.nodesource.com/setup_10.x | bash -

RUN apt-get install -y build-essential \
    nodejs \
    python-pip \
    zip

ENV GEM_HOME vendor/gem_home
ENV GEM_PATH vendor/gem_home
ENV RAILS_ENV production

RUN mkdir -p $GEM_HOME
RUN jruby -S gem install -i $GEM_HOME --no-rdoc --no-ri bundler -v 1.17.3
RUN bundle config BUNDLE_PATH $GEM_HOME
RUN bundle config BUNDLE_BIN bin
RUN bundle config BUNDLE_DISABLE_SHARED_GEMS true

RUN npm install -g yarn
RUN pip install awscli