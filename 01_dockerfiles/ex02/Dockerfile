FROM ruby

RUN apt-get update && apt-get install nodejs -y && \
	apt-get autoclean -y && apt-get clean -y && \
	gem install rails

ONBUILD COPY app /opt/app
ONBUILD WORKDIR /opt/app
ONBUILD EXPOSE 3000                                       
ONBUILD RUN bundle install
ONBUILD RUN rake db:migrate
ONBUILD RUN rake db:seed

WORKDIR /opt/app
