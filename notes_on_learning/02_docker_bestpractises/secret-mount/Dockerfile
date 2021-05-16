# syntax = docker/dockerfile:1.0-experimental
FROM alpine

# shows secret from default secret location
RUN --mount=type=secret,id=mysecret cat /run/secrets/mysecret

# shows secret from custom secret location
RUN --mount=type=secret,id=mysecret,dst=/foobar cat /foobar
