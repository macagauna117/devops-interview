FROM node:12-alpine

# Create app directory
WORKDIR /front

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm install

COPY . .

CMD [ "node", "./scripts/start.js" ]