FROM alpine:latest

RUN apk add --no-cache nodejs npm

WORKDIR /level1-project

COPY . /level1-project

RUN npm install

EXPOSE 80

ENTRYPOINT ["node"]

CMD ["index.js"]
