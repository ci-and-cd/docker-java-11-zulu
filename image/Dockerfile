FROM cirepo/nix:2.2.1-alpine-3.9


USER root


ARG IMAGE_ARG_FILESERVER

# https://www.azul.com/downloads/zulu/zulu-download-alpine/
ARG IMAGE_ARG_JAVA11_VERSION
ARG IMAGE_ARG_JAVA11_PACKAGE_DIGEST
ARG IMAGE_ARG_JAVA11_ZULU_VERSION
# "For Alpine Linux, builds are produced on a reduced schedule and may not be in sync with the other platforms."


ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8
ENV JAVA_HOME /usr/lib/jvm/zulu-11
ENV PATH=${JAVA_HOME}/bin:${PATH}


COPY --chown=root:root docker /data


# wget -q https://cdn.azul.com/zulu/bin/${ZULU_ARCH}
RUN set -eux \
  && ZULU_ARCH=zulu${IMAGE_ARG_JAVA11_ZULU_VERSION:-11.29.3}-ca-jdk${IMAGE_ARG_JAVA11_VERSION:-11.0.2}-linux_musl_x64.tar.gz \
  && INSTALL_DIR=$( dirname $JAVA_HOME ) \
  && BIN_DIR=/usr/bin \
  && MAN_DIR=/usr/share/man/man1 \
  && ZULU_DIR=$( basename ${ZULU_ARCH} .tar.gz ) \
  && if [[ ! -f /data/${ZULU_ARCH} ]]; then \
       ${ARIA2C_DOWNLOAD} -d /data -o ${ZULU_ARCH} \
       ${IMAGE_ARG_FILESERVER:-https://cdn.azul.com}/zulu/bin/${ZULU_ARCH}; \
     fi \
  && echo "${IMAGE_ARG_JAVA11_PACKAGE_DIGEST:-eba6b1ee7302a7e509a9f28ed4fa2359e29431aed8dcc43b1145a8c8a3847791} */data/${ZULU_ARCH}" | sha256sum -c - \
  && mkdir -p ${INSTALL_DIR} \
  && tar -xf /data/${ZULU_ARCH} -C ${INSTALL_DIR} \
  && rm -f /data/${ZULU_ARCH} \
  && mv ${INSTALL_DIR}/${ZULU_DIR} ${JAVA_HOME} \
  && cd ${BIN_DIR} && find ${JAVA_HOME}/bin -type f -perm -a=x -exec ln -s {} . \; \
  && mkdir -p ${MAN_DIR} \
  && cd ${MAN_DIR} && find ${JAVA_HOME}/man/man1 -type f -name "*.1" -exec ln -s {} . \; \
  \
# https://github.com/docker-library/openjdk/issues/212#issuecomment-420979840
# https://openjdk.java.net/jeps/341
  && java -Xshare:dump \
  \
# basic smoke test
  && java --version \
  && javac --version

USER alpine

# https://docs.oracle.com/javase/10/tools/jshell.htm
# https://docs.oracle.com/javase/10/jshell/
# https://en.wikipedia.org/wiki/JShell
CMD ["jshell"]
