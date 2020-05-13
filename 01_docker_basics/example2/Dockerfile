FROM debian:sid
RUN apt-get -y update
RUN apt-get install -y nano
RUN apt-get -y install iputils-ping && apt install -y netcat
CMD "nano" "/tmp/notes"

# So we make this image such that update the latest packages, install nano and we will make an image that when opened, runs a notes files to enter notes.