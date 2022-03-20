FROM axelor/aio-builder as builder

RUN mkdir -p /app/src
WORKDIR /app/src

RUN \
  set -ex && \
  git clone https://github.com/axelor/abs-webapp.git axelor-erp && \
  sed -e 's|git@github.com:|https://github.com/|' -i axelor-erp/.gitmodules && \
  cd axelor-erp && \
  git checkout 6.0 && \
  git submodule sync && \
  git submodule init && \
  git submodule update && \
  git submodule foreach git checkout 6.0 && \
  git submodule foreach git pull origin 6.0 && \
  sed -e 's|^application.theme.*|application.theme = modern|g' -i src/main/resources/application.properties && \
  ./gradlew --no-daemon -x test npm-build build

FROM axelor/aio-base
LABEL maintainer="Axelor <support@axelor.com>"

COPY --from=builder /app/src/axelor-erp/build/libs/axelor-erp-*.war $CATALINA_BASE/webapps/ROOT.war

CMD ["start"]
