# FROM ubuntu:16.
FROM node:10-alpine

COPY . /app

WORKDIR /app

RUN npm install

EXPOSE 81

CMD ["npm", "start"]