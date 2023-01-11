FROM node:18.10.0 as base
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . ./
EXPOSE 4435
CMD [ "npm","start" ]