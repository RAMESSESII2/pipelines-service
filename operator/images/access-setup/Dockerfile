#@FROM registry.access.redhat.com/ubi9/ubi-minimal
FROM registry.access.redhat.com/ubi9/ubi-minimal@sha256:61925d31338b7b41bfd5b6b8cf45eaf80753d415b0269fc03613c5c5049b879e
WORKDIR /
RUN mkdir /workspace && chmod 777 /workspace && chown 65532:65532 /workspace
ENV HOME /tmp/home
RUN mkdir $HOME && chmod 777 $HOME && chown 65532:65532 $HOME

RUN microdnf install -y \
      gzip-1.12 \
      tar-1.34 \
    && microdnf clean all

COPY shared /tmp/image-build/shared
WORKDIR /tmp/image-build/shared/hack
RUN set -x \
    && ./install.sh --bin jq,kubectl \
    && rm -rf /tmp/image-build

COPY operator/images/access-setup/content /opt/access-setup
RUN chmod 755 /opt/access-setup/bin/*.sh
ENV PATH="/opt/access-setup/bin:${PATH}"
USER 65532:65532
ENV WORK_DIR /workspace
VOLUME /workspace
WORKDIR /workspace
CMD ["/opt/access-setup/bin/setup_compute.sh"]
