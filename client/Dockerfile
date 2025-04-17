#Build stage
FROM node:16-alpine AS build
WORKDIR /usr/src/app
#Copy package files in package*.json
COPY package*.json ./
#Install dependencies
RUN npm ci
#Copy code
COPY . .
#Build the client application
RUN npm run build

#Using alpine version 3.21.3 with a minimal server
FROM alpine:3.21.3

#Install Node.js and a minimal web server,creates a single layer by chaining the instruction under single RUN command
# To enhance security create a non root system user to run you app
RUN apk add --no-cache nodejs npm && \
    npm install -g serve@14.2.4 && \
    npm cache clean --force && \
    addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001

WORKDIR /app
#copy files from build stage and assign ownership to nodejs user and group
COPY --from=build --chown=nodejs:nodejs /usr/src/app/build ./build
# Set environment variables for the production container
#switch context to non root user
USER nodejs

EXPOSE 3000

#Use serve installation to host the files
CMD ["serve","-s","build","-l","3000"]
