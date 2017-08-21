FROM pandorasystems/karaf:${KARAF_VERSION}

MAINTAINER Christopher Johnson <christopher_hanna.johnson@uni-leipzig.de>
LABEL description = "Provides a Karaf container configured with triplestore indexing and event broadcasting"

ENV MAVEN_REPO=/build/repository
ENV TOOLBOX_VERSION=${TOOLBOX_VERSION}
ENV CAMEL_VERSION=${CAMEL_VERSION}
ENV ACTIVEMQ_VERSION=${ACTIVEMQ_VERSION}
ENV FCREPO_HOST=${FCREPO_HOST}
ENV ACTIVEMQ_HOST=${ACTIVEMQ_HOST}
ENV FCREPO_PORT=${FCREPO_PORT}
ENV FCREPO_CONTEXT_PATH=${FCREPO_CONTEXT_PATH}
ENV FUSEKI_HOST=${FUSEKI_HOST}
ENV FUSEKI_PORT=${FUSEKI_PORT}
ENV DEBUG_PORT=${DEBUG_PORT}
ENV JOLOKIA_PORT=${JOLOKIA_PORT}
ENV REINDEXING_PORT=${REINDEXING_PORT}
ENV JAVA_DEBUG_PORT=${DEBUG_PORT}
ENV FUSEKI_BASEURI=http://${FUSEKI_HOST}:${FUSEKI_PORT}/fuseki/manifests
ENV FCREPO_BASEURI=http://${FCREPO_HOST}:${FCREPO_PORT}${FCREPO_CONTEXT_PATH}/rest

RUN mkdir -p ${mavenRepo}
ADD repository/ ${mavenRepo}

RUN bin/start && \
    sleep 10 && \
    bin/client -r 10 -d 5  "feature:repo-add mvn:cool.pandora.exts/messaging-karaf/${version}/xml/features" && \
    bin/client -r 10 -d 5  "feature:install fcrepo-service-activemq" && \
    bin/client -r 10 -d 5  "feature:install fcrepo-reindexing" && \
    bin/client -r 10 -d 5  "feature:install exts-connector-triplestore" && \
    bin/client -r 10 -d 5  "feature:install acrepo-connector-broadcast" && \
    bin/stop

COPY repository/cool/pandora/exts/*.cfg etc/
COPY repository/cool/pandora/exts/entrypoint.sh /entrypoint.sh

RUN chmod 700 /entrypoint.sh

EXPOSE ${REINDEXING_PORT}
EXPOSE ${JOLOKIA_PORT}

ENTRYPOINT [ "/entrypoint.sh" ]

CMD [ "server"]
