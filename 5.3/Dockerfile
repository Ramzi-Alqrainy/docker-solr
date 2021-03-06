
FROM    java:openjdk-8-jre
MAINTAINER  Ramzi ALqrainy "ramzi.alqrainy@gmail.com"

ENV SOLR_VERSION 5.3.1
ENV SOLR_SHA256 34ddcac071226acd6974a392af7671f687990aa1f9eb4b181d533ca6dca6f42d
ENV SOLR solr-$SOLR_VERSION
ENV SOLR_USER solr
ENV SOLR_UID 8983

RUN export DEBIAN_FRONTEND=noninteractive && \
  apt-get update && \
  apt-get -y install lsof && \
  rm -rf /var/lib/apt/lists/* && \
  mkdir -p /opt && \
  wget -nv --output-document=/opt/$SOLR.tgz http://archive.apache.org/dist/lucene/solr/$SOLR_VERSION/$SOLR.tgz && \
  echo "$SOLR_SHA256 *$SOLR.tgz" | (cd /opt; sha256sum -c -) && \
  tar -C /opt --extract --file /opt/$SOLR.tgz && \
  rm /opt/$SOLR.tgz && \
  mkdir -p /opt/$SOLR/server/solr/lib && \
  ln -s /opt/$SOLR /opt/solr
RUN sed --in-place -e 's/^    "$JAVA" "${SOLR_START_OPTS\[@\]}" $SOLR_ADDL_ARGS -jar start.jar "${SOLR_JETTY_CONFIG\[@\]}"/    exec "$JAVA" "${SOLR_START_OPTS[@]}" $SOLR_ADDL_ARGS -jar start.jar "${SOLR_JETTY_CONFIG[@]}"/' /opt/$SOLR/bin/solr
RUN groupadd -r $SOLR_USER && \
  useradd -r -u $SOLR_UID -g $SOLR_USER $SOLR_USER && \
  chown -R $SOLR_USER:$SOLR_USER /opt/solr /opt/$SOLR

EXPOSE 8983
WORKDIR /opt/solr
USER $SOLR_USER
CMD ["/bin/bash", "-c", "/opt/solr/bin/solr -f"]
