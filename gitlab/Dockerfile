ARG GITLAB_VERSION=latest

FROM "gitlab/gitlab-ce:${GITLAB_VERSION}"

LABEL maintainer="L-Chris@563303226@qq.com"

ARG GITLAB_RUNNER_VERSION=latest

ENV NODE_VERSION=12.2.0 \
    YARN_VERSION=1.16.0 \
    BINARY_PREFIX=/usr/bin

USER root

RUN echo 'Asia/Shanghai' > /etc/timezone && \
    dpkg-reconfigure -f noninteractive tzdata

RUN sed -i 's/http:\/\/archive\.ubuntu\.com\/ubuntu\//http:\/\/mirrors\.aliyun\.com\/ubuntu\//g' /etc/apt/sources.list

RUN curl https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh | bash && \
  echo "deb https://mirrors.tuna.tsinghua.edu.cn/gitlab-runner/ubuntu xenial main" > /etc/apt/sources.list.d/gitlab-runner.list && \
  echo "deb-src https://mirrors.tuna.tsinghua.edu.cn/gitlab-runner/ubuntu xenial main" >> /etc/apt/sources.list.d/gitlab-runner.list

RUN set -ex \
  && apt-get update \
  && apt-get install -y --no-install-recommends \
    binutils-gold \
    curl \
    g++ \
    gcc \
    gnupg \
    dirmngr \
    make \
    python \
    gitlab-runner=$GITLAB_RUNNER_VERSION \
  && rm -rf /var/lib/apt/lists/*

RUN ln -s /usr/bin/dirmngr /opt/gitlab/embedded/bin/dirmngr

RUN for key in \
    94AE36675C464D64BAFA68DD7434390BDBE9B9C5 \
    FD3A5288F042B6850C66B31F09FE44734EB7990E \
    71DCFD284A79C3B38668286BC97EC7A07EDE3FC1 \
    DD8F2338BAE7501E3DD5AC78C273792F7D83545D \
    C4F0DFFF4E8C1A8236409D08E73BC641CC11F4C8 \
    B9AE9905FFD7803F25714661B63B535A4C206CA9 \
    77984A986EBC2AA786BC0F66B01FBB92821C587A \
    8FCCA13FEF1D0C2E91008E09770F7A9A5AE15600 \
    4ED778F539E3634C779C87C6D7062848A1AB005C \
    A48C2BEE680E841632CD4E44F07496B3EB3C1762 \
    B9E2F5981AA6E0CD28160D9FF13993A75599653C \
  ; do \
    gpg --batch --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys "$key" || \
    gpg --batch --keyserver hkp://ipv4.pool.sks-keyservers.net --recv-keys "$key" || \
    gpg --batch --keyserver hkp://pgp.mit.edu:80 --recv-keys "$key" ; \
  done \
  && curl -fsSLO --compressed "https://npm.taobao.org/mirrors/node/v$NODE_VERSION/node-v$NODE_VERSION.tar.xz" \
  && curl -fsSLO --compressed "https://npm.taobao.org/mirrors/node/v$NODE_VERSION/SHASUMS256.txt.asc" \
  && gpg --batch --decrypt --output SHASUMS256.txt SHASUMS256.txt.asc \
  && grep " node-v$NODE_VERSION.tar.xz\$" SHASUMS256.txt | sha256sum -c - \
  && tar -xf "node-v$NODE_VERSION.tar.xz" \
  && cd "node-v$NODE_VERSION" \
  && ./configure \
  && make -j$(getconf _NPROCESSORS_ONLN) V= \
  && make install \
  && cd .. \
  && rm -Rf "node-v$NODE_VERSION" \
  && rm "node-v$NODE_VERSION.tar.xz" SHASUMS256.txt.asc SHASUMS256.txt

RUN for key in \
    6A010C5166006599AA17F08146C2130DFD2497F5 \
  ; do \
    gpg --batch --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys "$key" || \
    gpg --batch --keyserver hkp://ipv4.pool.sks-keyservers.net --recv-keys "$key" || \
    gpg --batch --keyserver hkp://pgp.mit.edu:80 --recv-keys "$key" ; \
  done \
  && curl -fsSLO --compressed "https://npm.taobao.org/mirrors/yarn/v$YARN_VERSION/yarn-v$YARN_VERSION.tar.gz" \
  && curl -fsSLO --compressed "https://npm.taobao.org/mirrors/yarn/v$YARN_VERSION/yarn-v$YARN_VERSION.tar.gz.asc" \
  && gpg --batch --verify yarn-v$YARN_VERSION.tar.gz.asc yarn-v$YARN_VERSION.tar.gz \
  && mkdir -p /opt \
  && tar -xzf yarn-v$YARN_VERSION.tar.gz -C /opt/ \
  && ln -s /opt/yarn-v$YARN_VERSION/bin/yarn ${BINARY_PREFIX}/yarn \
  && ln -s /opt/yarn-v$YARN_VERSION/bin/yarnpkg ${BINARY_PREFIX}/yarnpkg \
  && rm yarn-v$YARN_VERSION.tar.gz.asc yarn-v$YARN_VERSION.tar.gz

RUN npm config set registry http://npm.ekwing.com \
  && npm config set sass_binary_site http://cdn.npm.taobao.org/dist/node-sass \
  && yarn config set registry http://npm.ekwing.com \
  && yarn config set sass_binary_site http://cdn.npm.taobao.org/dist/node-sass
