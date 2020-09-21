FROM node:14.11.0-alpine3.11 AS builder

WORKDIR /app

COPY package.json .
COPY yarn.lock .
COPY internals/scripts ./internals/scripts
RUN npm i yarn
RUN yarn install --production 
COPY . .
RUN yarn build
RUN ls -la

# -------------

FROM node:14.11.0-alpine3.11

WORKDIR /app

COPY --from=builder /app/build .
COPY --from=builder /app/package.json .

COPY --from=builder /app/internals/scripts /internals
COPY --from=builder /app/server .

ENTRYPOINT ["yarn", "run", "start:prod"]