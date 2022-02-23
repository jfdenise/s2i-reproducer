FROM quay.io/jfdenise/wildfly-s2i-jdk11:latest AS build
USER root
RUN echo "[JFDENISE]/tmp content"
RUN ls -l /tmp
RUN echo "[JFDENISE] . content"
RUN ls -l .
RUN echo "[JFDENISE] Then copy all"
COPY . /tmp/src/
RUN echo "[JFDENISE]After copy tmp content"
RUN ls /tmp
RUN chown -R jboss:root /tmp/src/ && chmod -R ug+rwX /tmp/src/
USER jboss

RUN /usr/local/s2i/assemble

FROM quay.io/jfdenise/wildfly-runtime-jdk11:latest
COPY --from=build /opt/server $JBOSS_HOME
USER root
RUN chown -R jboss:root $JBOSS_HOME && chmod -R ug+rwX $JBOSS_HOME
USER jboss
