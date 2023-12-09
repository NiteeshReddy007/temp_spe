FROM node:alpine AS build
WORKDIR /app

COPY package*.json /app/
RUN npm install --silent
COPY . .

EXPOSE 5001
CMD ["npm", "start"]
