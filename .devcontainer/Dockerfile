## Will create a node environment in the container
FROM node:18-alpine AS base
## Install pnpm globally
RUN npm i -g pnpm

FROM base AS dependencies
## Will create a directory app and switch to that directory
WORKDIR /app
## Copies package.json file to /app directory
COPY package.json pnpm-lock.yaml ./
## Runs pnpm install to create node_modules for your app
RUN pnpm install

FROM base as development
ENV NODE_ENV=development
WORKDIR /app
## Copies the source code to /app directory
COPY . .
## Copies the node_modules from the dependencies stage to /app directory
## The purpose is to avoid installing the dependencies again & this will save time and space
COPY --from=dependencies /app/node_modules ./node_modules
## Exposes the port to access the app from outside the container i.e from the browser
EXPOSE 5173
## Executes npm run dev to start the server
CMD ["pnpm", "dev"]

FROM base as build
WORKDIR /app
## Copies the source code to /app directory
COPY . .
## Copies the node_modules from the dependencies stage to /app directory
## The purpose is to avoid installing the dependencies again & this will save time and space
COPY --from=dependencies /app/node_modules ./node_modules
RUN pnpm build
RUN pnpm prune --prod

FROM base AS deploy
WORKDIR /app
COPY --from=build /app/dist/ ./dist/
COPY --from=build /app/node_modules ./node_modules

CMD ["node", "dist/main.js"]