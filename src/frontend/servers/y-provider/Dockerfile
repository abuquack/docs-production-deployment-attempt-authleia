FROM node:20-alpine AS y-provider-builder

WORKDIR /home/frontend/

COPY ./src/frontend ./

RUN yarn install

WORKDIR /home/frontend/servers/y-provider
RUN yarn build

FROM node:20-alpine AS y-provider

WORKDIR /home/frontend/

COPY ./src/frontend ./

WORKDIR /home/frontend/servers/y-provider
COPY --from=y-provider-builder \
    /home/frontend/servers/y-provider/dist \
    ./dist

RUN NODE_ENV=production yarn install --frozen-lockfile

# Remove npm, contains CVE related to cross-spawn and we don't use it.
RUN rm -rf /usr/local/bin/npm /usr/local/lib/node_modules/npm

# Un-privileged user running the application
ARG DOCKER_USER
USER ${DOCKER_USER}

# Copy entrypoint
COPY ./docker/files/usr/local/bin/entrypoint /usr/local/bin/entrypoint
ENTRYPOINT [ "/usr/local/bin/entrypoint" ]

WORKDIR /home/frontend/servers/y-provider

CMD ["yarn", "start"]
