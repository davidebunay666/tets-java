ARG CONTAINER
FROM $CONTAINER

USER root
RUN rm -f /etc/localtime && ln -s /usr/share/zoneinfo/America/Guayaquil /etc/localtime
COPY *.jar /deployments/
COPY configuration/devConfig/application.properties /deployments/data/

RUN chgrp -R 0 /deployments && chmod -R g=u /deployments
USER 1001

CMD ["java","-jar", "/deployments/demo-0.0.1.jar", "--spring.config.location=file:///deployments/data/application.properties"]

