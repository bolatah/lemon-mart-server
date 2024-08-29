# Use a specific Node.js version that aligns with your application’s requirements
FROM node:20-alpine

# Install required packages
RUN apk add --update --no-progress make python3 bash

# Add dumb-init for handling PID 1
ADD https://github.com/Yelp/dumb-init/releases/download/v1.2.5/dumb-init_1.2.5_x86_64 /usr/local/bin/dumb-init
RUN chmod +x /usr/local/bin/dumb-init

# Set working directory
WORKDIR /home/node/app

# Copy package files with appropriate ownership and install dependencies
# Switch to root temporarily to set directory permissions
USER root

# Create the working directory and set ownership to the node user
RUN mkdir -p /home/node/app && chown -R node:node /home/node/app

# Switch to the node user to ensure all operations run with node privileges
USER node

# Copy package files with appropriate ownership
COPY --chown=node:node package*.json ./

# Install production dependencies as the node user
RUN npm install --omit=dev --ignore-scripts

# Copy the rest of the application code with ownership
COPY --chown=node:node dist dist

# Environment variables
ENV HOST "0.0.0.0"
ENV PORT 3000

# Expose the port your app runs on
EXPOSE 3000

# Start the application using dumb-init to handle PID 1
ENTRYPOINT ["dumb-init", "--"]
CMD ["node", "dist/src/index"]
