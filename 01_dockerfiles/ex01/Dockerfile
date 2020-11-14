FROM ubuntu

RUN apt update && apt upgrade -y && apt install wget tar -y

EXPOSE 10011 30033 9987/udp

RUN wget http://dl.4players.de/ts/releases/3.13.1/teamspeak3-server_linux_amd64-3.13.1.tar.bz2 \
	&& tar -xvf teamspeak3-server_linux_amd64-3.13.1.tar.bz2

WORKDIR teamspeak3-server_linux_amd64

RUN touch .ts3server_license_accepted

ENTRYPOINT ./ts3server_minimal_runscript.sh

##docker run --rm -p 9987:9987/udp -p 10011:10011 -p 30033:30033 my_teamspeak
