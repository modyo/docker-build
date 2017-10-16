FROM jruby:1.7.27

# Change packages source to cloudfront
RUN sed 's@deb.debian.org@cloudfront.debian.net@' -i /etc/apt/sources.list
RUN sed 's@security.debian.org@cloudfront.debian.net/debian-security@' -i /etc/apt/sources.list
RUN export DEBIAN_VERSION=$(awk -F"[)(]+" '/VERSION=/ {print $2}' /etc/os-release) && sed 's@deb.debian.org@cloudfront.debian.net@' -i /etc/apt/sources.list.d/${DEBIAN_VERSION}-backports.list || echo 'Missing ${DEBIAN_VERSION}-backports.list file. Skipping'

# Install dependencies
RUN apt-get update -qq && apt-get install -y build-essential
RUN apt-get install -y git

# Install Zip.
RUN apt-get install zip

# Install node js.
RUN curl -sL https://deb.nodesource.com/setup_6.x | bash -
RUN apt-get install -y nodejs

RUN apt-get dist-upgrade -y
RUN apt-get install imagemagick -y

# Docker image won't build on Teamcity without this
ENV TERM=xterm

ENV BUILD_NUMBER $BUILD_NUMBER
ENV RAILS_ENV ${RAILS_ENV:-staging} # staging is the default if no environment is set
ENV SSH_AUTH_SOCK $SSH_AUTH_SOCK
ENV CLIENT_PREFIX $APP_PREFIX
ENV CLIENT_TOKEN $CLIENT_TOKEN
ENV ARTIFACTORY_USER $ARTIFACTORY_USER
ENV ARTIFACTORY_KEY $ARTIFACTORY_KEY

# Jruby opts for increased memory
ENV JRUBY_OPTS '-J-client -J-Xss2m -J-Xms1536m -J-Xmx1536m -J-XX:+CMSClassUnloadingEnabled -J-XX:+UseConcMarkSweepGC'

# Set an environment variable where the Rails app is installed to inside of Docker image:
ENV RAILS_ROOT /var/www/modyo
RUN mkdir -p $RAILS_ROOT

# Set working directory, where the commands will be ran:
WORKDIR $RAILS_ROOT

# Force .bundle folder creation inside project
ENV BUNDLE_APP_CONFIG './.bundle'

