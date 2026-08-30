FROM ubuntu

RUN apt update

WORKDIR /app

COPY hello.txt /app/

ENV APP_NAME="MLOps-Docker"

EXPOSE 8000

CMD ["cat", "/app/hello.txt"]