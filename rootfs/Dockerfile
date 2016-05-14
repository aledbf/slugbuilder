FROM registry.soficom.cl/soficom/cedarish:git-89eb174

RUN mkdir /app
RUN addgroup --quiet --gid 2000 slug && \
  useradd slug --uid=2000 --gid=2000 --home-dir /app --no-create-home

ADD ./bin /bin
ADD https://dl.bintray.com/deis/deisci/objstorage-90ca1f4-linux-amd64 /bin/objstorage
RUN chmod +x /bin/objstorage
RUN chown -R slug:slug /app
RUN chown slug:slug /bin/get_object
RUN chown slug:slug /bin/put_object
RUN chown slug:slug /bin/objstorage

RUN mkdir -p /tmp/build && chmod 777 -R /tmp/build

COPY libxl-lin-3.6.5.tar.gz /tmp/libxl-lin-3.6.5.tar.gz
ENV NODE_LIBXL_SDK_ARCHIVE /tmp/libxl-lin-3.6.5.tar.gz

RUN chmod 755 /tmp/libxl-lin-3.6.5.tar.gz

USER slug
ENV HOME /app
ADD ./builder/ /tmp/builder
RUN /tmp/builder/install-buildpacks

ENTRYPOINT ["/tmp/builder/build.sh"]

ENV DEIS_RELEASE 2.0.0-dev
