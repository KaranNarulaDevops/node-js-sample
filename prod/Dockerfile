FROM node:10-alpine

WORKDIR /app

COPY ../package*.json ./app
COPY ../. .

RUN npm install

EXPOSE 4000

CMD [ "npm", "start" ]
