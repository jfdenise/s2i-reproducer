FROM quay.io/jfdenise/wildfly-s2i-jdk11:latest AS build
USER root

COPY . /tmp/src/
RUN chown -R jboss:root /tmp/src/ && chmod -R ug+rwX /tmp/src/
USER jboss
RUN ls /tmp/src
ARG MAVEN_LOCAL_REPO
ARG MAVEN_ARGS_APPEND
ARG MAVEN_OPTS
ARG GALLEON_PROVISION_LAYERS
ARG GALLEON_PROVISION_FEATURE_PACKS

RUN /usr/local/s2i/assemble

FROM quay.io/jfdenise/wildfly-runtime-jdk11:latest
COPY --from=build /opt/server $JBOSS_HOME
USER root
RUN chown -R jboss:root $JBOSS_HOME && chmod -R ug+rwX $JBOSS_HOME
USER jboss
