FROM quay.io/ibmz/compose:1.27.4

COPY docker-compose.yml /home/

ENV COMPOSE_FILE=/home/docker-compose.yml

CMD ["docker-compose", "up"]
