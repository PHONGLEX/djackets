FROM node:15.13-alpine

# make the 'app' folder the current working directory
WORKDIR /vue/

COPY package.json ./

RUN npm cache verify

# install project dependencies
RUN npm install

# copy project files and folders to the current working directory (i.e. 'app' folder)
COPY . .

