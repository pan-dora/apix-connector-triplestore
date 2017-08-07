FROM pandorasystems/karaf:4.0.9

MAINTAINER Christopher Johnson <chjohnson39@gmail.com>
LABEL description = "Provides a Karaf container configured with Camel Toolbox features"

# The version of TOOLBOX features being included in the image
ENV TOOLBOX_VERSION=LATEST \
    CAMEL_VERSION=2.19.2 \
    ACTIVEMQ_VERSION=LATEST \
    FCREPO_HOST=fcrepo \
    ACTIVEMQ_HOST=activemq \
    FCREPO_PORT=8080 \
    FCREPO_CONTEXT_PATH=/fcrepo \
    FUSEKI_HOST=fuseki \
    FUSEKI_PORT=3030 \
    DEBUG_PORT=5005  \
    JOLOKIA_PORT=8181 \
    REINDEXING_PORT=9080

RUN mkdir -p ${MAVEN_REPO}
ADD repository/ ${MAVEN_REPO}

RUN echo "pandora=mvn:cool.pandora/messaging-karaf/${PANDORA_EXTS_VERSION}/xml/features" >> etc/org.apache.karaf.features.repos.cfg

RUN sed -e "s:^\(    mvn\:org.apache.karaf.features/enterprise/${KARAF_VERSION}/xml/features\):\1, \\\
    mvn\:cool.pandora/messaging-karaf/${PANDORA_EXTS_VERSION}/xml/features \\\:" \
        -i etc/org.apache.karaf.features.cfg

RUN \
  echo "eval 'alias mvn=\"mvn -Dmaven.repo.local=\${MAVEN_REPO}\"'" \
    >> /etc/skel/.bashrc && \
  echo "eval 'alias mvn=\"mvn -Dmaven.repo.local=\${MAVEN_REPO}\"'" \
    >> ~/.bashrc

RUN bin/start && \
    sleep 10 && \
    bin/client -r 10 -d 5  "feature:repo-add mvn:org.fcrepo.camel/toolbox-features/${TOOLBOX_VERSION}/xml/features" && \
    bin/client -r 10 -d 5  "feature:repo-add camel ${CAMEL_VERSION}" && \
    bin/client -r 10 -d 5  "feature:repo-add activemq ${ACTIVEMQ_VERSION}" && \
    bin/client -r 10 -d 5  "feature:install fcrepo-service-activemq" && \
    bin/client -r 10 -d 5  "feature:install fcrepo-reindexing" && \
    bin/client -r 10 -d 5  "feature:repo-add mvn:cool.pandora/messaging-karaf/LATEST/xml/features" && \
    bin/client -r 10 -d 5  "feature:install exts-connector-triplestore" && \
    bin/client -r 10 -d 5  "feature:repo-add mvn:edu.amherst.acdc/acrepo-karaf/LATEST/xml/features" && \
    bin/client -r 10 -d 5  "feature:install acrepo-connector-broadcast" && \
    bin/stop


ENV JAVA_DEBUG_PORT=${DEBUG_PORT} \
    FUSEKI_BASEURI=http://${FUSEKI_HOST}:${FUSEKI_PORT}/fuseki/manifests \
    FCREPO_BASEURI=http://${FCREPO_HOST}:${FCREPO_PORT}${FCREPO_CONTEXT_PATH}/rest

# Copy Karaf feature configuration files into the image
COPY repository/cool/pandora/*.cfg etc/

# Update the Karaf logging configuration so that `bin/karaf` will echo logging
# to the console, viewable by `docker logs`

RUN sed -e "s/osgi:/stdout, osgi:/" -i etc/org.ops4j.pax.logging.cfg

COPY repository/cool/pandora/entrypoint.sh /entrypoint.sh

RUN chmod 700 /entrypoint.sh

EXPOSE ${REINDEXING_PORT}
EXPOSE ${JOLOKIA_PORT}
ENTRYPOINT [ "/entrypoint.sh" ]

# Launch Karaf with no console, and debugging enabled by default.
CMD [ "server", "debug" ]
