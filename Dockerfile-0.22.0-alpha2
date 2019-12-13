FROM openjdk:11.0.5-jdk-stretch

ARG DISTBALL

ENV ZB_HOME=/usr/local/zeebe \
    ZEEBE_LOG_LEVEL=info \
    ZEEBE_GATEWAY_HOST=0.0.0.0 \
    ZEEBE_STANDALONE_GATEWAY=false
ENV PATH "${ZB_HOME}/bin:${PATH}"
ENV ZEEBE_VERSION=0.22.0-alpha2
ADD https://github.com/krallin/tini/releases/download/v0.18.0/tini /bin/tini
RUN chmod +x /bin/tini
RUN mkdir  -p ${ZB_HOME}
RUN wget -O ${ZB_HOME}/zeebe-distribution-${ZEEBE_VERSION}.tar.gz https://github.com/zeebe-io/zeebe/releases/download/${ZEEBE_VERSION}/zeebe-distribution-${ZEEBE_VERSION}.tar.gz 
RUN tar xfvz ${ZB_HOME}/*.tar.gz --strip 1 -C ${ZB_HOME}/ && rm ${ZB_HOME}/*.tar.gz

WORKDIR ${ZB_HOME}
EXPOSE 26500 26501 26502
VOLUME ${ZB_HOME}/data

COPY docker/utils/startup.sh /usr/local/bin
COPY --from=hengyunabc/arthas:latest /opt/arthas /opt/arthas
RUN apt-get --purge remove -y --auto-remove curl

ENTRYPOINT ["tini", "--", "/usr/local/bin/startup.sh"]
