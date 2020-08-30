FROM node:lts-alpine3.10 AS build

WORKDIR /office-service

COPY package*.json ./

RUN npm install

COPY . . 

EXPOSE 80 
CMD node index.js
