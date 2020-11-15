FROM ruby:2.5

RUN	apt-get update -y
RUN	gem install rails && gem install bundler
RUN	apt-get -y install build-essential && apt-get -y install libpq-dev && apt-get -y install nodejs && apt-get -y install sqlite3

ONBUILD COPY app /opt/app
ONBUILD WORKDIR /opt/app

ONBUILD EXPOSE	3000

ONBUILD RUN	bundle install --without production

ONBUILD RUN bundle exec rake db:migrate
ONBUILD RUN bundle exec rake db:seed
