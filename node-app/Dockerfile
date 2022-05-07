
# Build the first container, that we're gonna throw away
FROM node:12-stretch AS build
WORKDIR /build

# We run this first so that we can take advantage of layers:
# If you've changed sth in the app, the node modules still won't be
# rebuilt at RUN npm ci
COPY package-lock.json package.json ./

# npm ci will adhere to whatever is in the package-lock.json that is copied to the container
RUN npm ci

# Instead of COPY you can also use ADD. But ADD does more:
# it can also download a file off the network and automatically unzip a file.
# Better to use COPY if you don't need this
COPY . .


# This is what you would do if you would make alpine node from scratch and without npm
FROM alpine:3.10
RUN apk add --update nodejs
RUN addgroup -S node && adduser -S node -G node

# Better not to put the server directly in the root folder.
USER node



# This will both work, but in the tutorial only the second command works, since 
# the first one with WORKDIR will cause permission errors
# This actually also automatically switches to this directory
# WORKDIR /home/node/src
RUN mkdir home/node/src

# Let it be owned by the user node in the group node
# This is good because otherwise the owner would automatically be root
# The name 'build' refers to alias of first container
COPY --from=build --chown=node:node /build .

CMD ["node", "index.js"]