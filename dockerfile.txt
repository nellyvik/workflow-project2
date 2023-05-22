FROM node:15-alpine AS build

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
COPY package*.json ./

RUN npm install

# If you are building your code for production
RUN npm ci --omit=dev

# Bundle app source
COPY . .

RUN npm run build --production

FROM node:16-alpine
WORKDIR /app
COPY --from=build /usr/src/app .
EXPOSE 3000
# ENTRYPOINT [ "/bin/sh" ]
ENTRYPOINT [ "serve" ]
# RUN npm install -g --force npx
RUN npm install -g serve
CMD [ "/app/build"]