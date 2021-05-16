FROM ubuntu:18.04 as intermediate

WORKDIR /app
RUN echo "build" > /app/app.txt

FROM ubuntu:18.04
WORKDIR /app
COPY --from=intermediate /app .
