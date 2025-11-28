# Use Node.js official image
FROM node:18-alpine

WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./
RUN npm install --production

# Copy the backend code
COPY . .

# Expose the port your server uses
EXPOSE 5000

# Start the backend server
CMD ["node", "server.js"]
