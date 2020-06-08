# NODE-VERSION: LTS
FROM node:lts-alpine as base
ENV NODE_ENV=production
WORKDIR /app
COPY package*.json ./
RUN npm install --only=production
ENV PATH /app/node_modules/.bin:$PATH

FROM base as dev
ENV NODE_ENV=development
RUN npm install --only=development
CMD npm run dev

# NODE-VERSION: The most recent version (current)
FROM node:current-alpine as base
ENV NODE_ENV=test
RUN npm install
RUN npm audit
CMD npm test

FROM dev as build
COPY . .
RUN npm build

FROM base as prod
COPY --from=build /app/dist/ .
CMD npm start