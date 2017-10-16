FROM jruby:9.1.9.0

# Change packages source to cloudfront
RUN sed 's@deb.debian.org@cloudfront.debian.net@' -i /etc/apt/sources.list
RUN sed 's@security.debian.org@cloudfront.debian.net/debian-security@' -i /etc/apt/sources.list
RUN export DEBIAN_VERSION=$(awk -F"[)(]+" '/VERSION=/ {print $2}' /etc/os-release) && sed 's@deb.debian.org@cloudfront.debian.net@' -i /etc/apt/sources.list.d/${DEBIAN_VERSION}-backports.list || echo 'Missing ${DEBIAN_VERSION}-backports.list file. Skipping'

# Install dependencies
RUN apt-get update -qq && apt-get install -y build-essential

# Install Zip.
RUN apt-get install zip

# Install node js.
RUN curl -sL https://deb.nodesource.com/setup_6.x | bash -
RUN apt-get install -y nodejs

RUN apt-get dist-upgrade -y
RUN apt-get install ImageMagick -y
RUN apt-get install libicu-dev -y
RUN apt-get install netcat -y

# Install Amazon AWS-CLI
RUN apt-get -y install python-dev
RUN apt-get -y install python-pip
RUN pip install awscli

# Install Docker
RUN curl -sSL https://get.docker.com/ | sh

# Mariadb asks for this constant to be defined.
ENV TERM=xterm

# Build number, defined by Teamcity
ENV BUILD_NUMBER ${BUILD_NUMBER}
ENV SSH_AUTH_SOCK $SSH_AUTH_SOCK

# Set an environment variable where the Rails app is installed to inside of Docker image:
ENV RAILS_ROOT /var/www/modyo
ENV RAILS_ENV=production
RUN mkdir -p $RAILS_ROOT

# JRUBY OPTIONS.
ENV JRUBY_OPTS='-J-Xss1m -J-Xms2048m -J-Xmx2048m --1.9 -J-XX:ReservedCodeCacheSize=128m -J-XX:+CMSClassUnloadingEnabled -J-XX:+UseConcMarkSweepGC'

# Set working directory, where the commands will be ran:
WORKDIR $RAILS_ROOT

