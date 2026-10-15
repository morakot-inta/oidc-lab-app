# Stage 1: stamp the version (commit SHA) into the page
FROM alpine:3.20 AS build
ARG VERSION=dev
COPY index.html /src/index.html
RUN sed -i "s/__VERSION__/${VERSION}/" /src/index.html

# Stage 2: serve it with nginx as a non-root user (listens on 8080)
FROM nginxinc/nginx-unprivileged:1.27-alpine
COPY --from=build /src/index.html /usr/share/nginx/html/index.html
EXPOSE 8080
