
# Build the first container, that we're gonna throw away
FROM node:12-stretch

RUN mkdir home/node/src

# We run this first so that we can take advantage of layers:
# If you've changed sth in the app, the node modules still won't be
# rebuilt at RUN npm ci
COPY package-lock.json package.json ./

# npm ci will adhere to whatever is in the package-lock.json that is copied to the container
RUN npm ci

USER node

# Instead of COPY you can also use ADD. But ADD does more:
# it can also download a file off the network and automatically unzip a file.
# Better to use COPY if you don't need this
COPY --chown=node:node . .

CMD ["node", "index.js"]