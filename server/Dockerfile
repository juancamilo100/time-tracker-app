FROM node:14.11.0-alpine3.11 AS builder

WORKDIR /app

COPY . .
RUN npm install --only=production 
RUN cp -R node_modules prod_node_modules
RUN npm install
RUN npm run build

# -------------

FROM node:14.11.0-alpine3.11

ARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}
ENV DOCKERIZE_VERSION v0.6.1
RUN apt-get update && apt-get install -y wget
RUN wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && tar -C /usr/local/bin -xzvf dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && rm dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz
WORKDIR /app

COPY --from=builder /app/prod_node_modules node_modules
COPY --from=builder /app/build build
COPY --from=builder /app/swagger.yml .
EXPOSE 9000:9000
CMD [ "node", "./build/bin/www.js" ]