FROM ubuntu:latest
RUN apt-get update
RUN apt-get -y install curl
RUN curl https://google.com | wc -c > google-size
# Just count how big is google homepage in characters. When project runs we want it to display the information 
ENTRYPOINT echo google is this big; cat google-size
