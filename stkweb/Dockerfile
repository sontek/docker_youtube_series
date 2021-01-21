FROM node

WORKDIR /srv
COPY package.json yarn.lock .
RUN yarn
COPY . .

EXPOSE 3000
CMD ["yarn", "start"]
