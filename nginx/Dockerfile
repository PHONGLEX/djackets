# build stage
FROM node:15.13-alpine as build-stage
WORKDIR /app/
COPY vue/package.json /app/
RUN npm cache verify
RUN npm install
COPY vue /app/
RUN npm run build

# production stage
FROM nginx:latest as production-stage
COPY nginx/nginx-setup.conf /etc/nginx/nginx.conf
COPY django/static /usr/src/app/static/
COPY django/media /usr/src/app/media/
COPY --from=build-stage /app/dist /dist/
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
