FROM python:3

ARG PROJECT_REFERENCE=test

# Base service setup
RUN apt-get update && \
  apt-get install -y \
  nginx \
  supervisor

# Artifact copying in to the container
COPY docker/uwsgi_params /etc/nginx/uwsgi_params
COPY docker/nginx.conf /etc/nginx/nginx.conf
COPY docker/uwsgi.ini /root/uwsgi.ini
COPY docker/supervisord.conf /root/supervisord.conf
COPY . /usr/src/app

WORKDIR /usr/src/app
RUN pip install --user \
    Django \
    pipenv \
    uwsgi \
    && \
    /root/.local/bin/pipenv install


# Customise for this project
RUN sed -i "s/###PROJECT_REFERENCE###/$PROJECT_REFERENCE/g" /root/uwsgi.ini && \
    sed -i "s/###PROJECT_REFERENCE###/$PROJECT_REFERENCE/g" /etc/nginx/nginx.conf

# Exceution preparation
EXPOSE 80

CMD ["/usr/bin/supervisord", "-c", "/root/supervisord.conf"]
