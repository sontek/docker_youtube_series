FROM node:alpine as dev

WORKDIR /srv
COPY package.json yarn.lock ./
RUN yarn
COPY . .
RUN yarn build
EXPOSE 3000
CMD ["yarn", "start"]

FROM nginx:alpine as prod
WORKDIR /srv
COPY --from=dev /srv/build /usr/share/nginx/html
COPY default.conf /etc/nginx/conf.d/
