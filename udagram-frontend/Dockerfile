FROM beevelop/ionic:v2023.10.1 AS ionic

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm ci -f

COPY . .
RUN ionic build

FROM nginx:alpine3.18

COPY --from=ionic  /usr/src/app/www /usr/share/nginx/html