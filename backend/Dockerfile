#Build stage
FROM node:23-alpine3.20 AS build

WORKDIR /usr/src/app

#Copy package files and only install necessary dependancies
COPY package*.json ./
RUN npm ci
#Copy necessary source files
COPY . .

#Using alpine version 3.21.3
FROM alpine:3.21.3

#Install node.js runtime and create non root user
RUN apk add --no-cache nodejs && \
    addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001

WORKDIR /app
# Copy only the minimal required files from build stage
COPY --from=build --chown=nodejs:nodejs /usr/src/app/package.json ./
COPY --from=build --chown=nodejs:nodejs /usr/src/app/node_modules ./node_modules

# Copy application code - adjust these paths based on your project structure
COPY --from=build --chown=nodejs:nodejs /usr/src/app/*.js ./
COPY --from=build --chown=nodejs:nodejs /usr/src/app/routes ./routes
COPY --from=build --chown=nodejs:nodejs /usr/src/app/models ./models

USER nodejs

EXPOSE 5000
# Start application
CMD ["node", "server.js"]
