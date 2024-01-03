FROM node:21.5-alpine3.19 as build

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm ci -f && npm install -g @ionic/cli

COPY . .
RUN ionic build

FROM nginx:alpine3.18

COPY --from=build  /usr/src/app/www /usr/share/nginx/html