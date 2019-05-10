FROM alpine:3.9

ENV HUGO_VERSION 0.55.4
ENV SOURCE_DIR /src
ENV SITE_DIR /srv
ENV SITE_URL http://localhost:1313

# Supporting packages
# RUN echo "http://dl-cdn.alpinelinux.org/alpine/edge/main" >> /etc/apk/repositories && \
#     echo "http://dl-cdn.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories && \
#     apk --update add \
#     curl \
#     g++ \
#     gcc \
#     git \
#     go \
#     libc-dev \
#     make \
#     musl-dev \
#     nodejs \
#     npm && \
#     npm i -g postcss-cli

# Adding GO paths to environment
COPY .profile /etc/.profile
CMD sh
# # Install Hugo Static Site Generator
# WORKDIR /tmp
# RUN git clone --single-branch --branch release-${HUGO_VERSION} https://github.com/gohugoio/hugo.git && \
#   cd hugo && \
#   go install --tags extended
#
# # Have the site built so that CMD can be simple run
# WORKDIR ${SOURCE_DIR}
# ONBUILD COPY site/ ${SOURCE_DIR}
# ONBUILD RUN hugo -d ${SITE_DIR}
#
# expose 1313
# CMD hugo server -b ${SITE_URL} --bind=0.0.0.0
