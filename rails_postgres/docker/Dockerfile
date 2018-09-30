FROM ruby:2.5

# throw errors if Gemfile has been modified since Gemfile.lock
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs \
    postgresql-client imagemagick

ENV INSTALL_PATH /usr/src/app

WORKDIR $INSTALL_PATH

COPY Gemfile Gemfile.lock ./

ENV BUNDLE_PATH=/bundle \
    BUNDLE_BIN=/bundle/bin \
    GEM_HOME=/bundle
ENV PATH="${BUNDLE_BIN}:${PATH}"
RUN bundle install

COPY . .

# Add bundle entry point to handle bundle cache
COPY docker/docker-entrypoint.sh /
RUN chmod +x /docker-entrypoint.sh
ENTRYPOINT ["/docker-entrypoint.sh"]
# ----------------------------------------

# Add bundle wait-for-postgres
COPY docker/wait-for-postgres.sh /
RUN chmod +x /wait-for-postgres.sh
# ----------------------------------------

# CMD ["./your-daemon-or-script.rb"]