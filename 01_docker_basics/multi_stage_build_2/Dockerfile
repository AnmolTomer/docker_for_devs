FROM ubuntu:latest as builder
RUN apt-get update
RUN apt-get -y install curl
RUN curl https://www.google.com | wc -c > google-size

FROM alpine
COPY --from=builder /google-size /google-size

ENTRYPOINT echo google is this big;cat google-size