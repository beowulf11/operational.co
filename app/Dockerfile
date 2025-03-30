FROM node:20 AS build

ARG VITE_API_URL
ARG VITE_APP_URL

WORKDIR /monorepo
COPY ./package.json .
COPY ./package-lock.json .
COPY ./packages ./packages
RUN mkdir app
COPY ./app/package.json ./app
RUN npm install

WORKDIR /monorepo/app
COPY ./app/package.json .
RUN npm install
COPY ./app .
RUN npm run build

FROM nginx:stable-alpine as production-stage
COPY --from=build /monorepo/app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
