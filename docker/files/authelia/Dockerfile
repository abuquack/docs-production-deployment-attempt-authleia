# Custom Authelia production image (optional, for custom config)
FROM authelia/authelia:latest

# Copy Authelia config and secrets if needed
COPY ./config /config
# COPY ./secrets /secrets

# If using secrets as files, ensure permissions are correct
# RUN chmod -R 600 /secrets

CMD ["authelia"]
