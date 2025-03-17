# good-container
FROM ubuntu:latest

RUN apt-get update
RUN apt-get install -y netcat-openbsd
RUN apt-get install -y docker.io

RUN useradd -m appuser

USER appuser

ENTRYPOINT ["sleep", "infinity"]
