FROM container-registry.oracle.com/os/oraclelinux:9-slim
RUN microdnf install -y --enablerepo ol9_appstream fontconfig ca-certificates curl openssl tzdata grafana-10.2.6 && \
    microdnf clean all
COPY LICENSE.txt SECURITY.md /usr/share/licenses/
COPY olm/NOTICE.md /usr/share/licenses/NOTICE.md
COPY olm/run.sh /run.sh

ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin \
    GF_PATHS_CONFIG="/etc/grafana/grafana.ini" \
    GF_PATHS_DATA="/var/lib/grafana" \
    GF_PATHS_HOME="/usr/share/grafana" \
    GF_PATHS_LOGS="/var/log/grafana" \
    GF_PATHS_PLUGINS="/var/lib/grafana/plugins" \
    GF_PATHS_PROVISIONING="/etc/grafana/provisioning"

WORKDIR $GF_PATHS_HOME

RUN    mkdir -p "$GF_PATHS_HOME/.aws" &&\
       mkdir -p "$GF_PATHS_PROVISIONING/datasources" \
                "$GF_PATHS_PROVISIONING/dashboards" \
	              "$GF_PATHS_PROVISIONING/notifiers" \
                "$GF_PATHS_LOGS" \
                "$GF_PATHS_PLUGINS" \
                "$GF_PATHS_DATA" &&\
       cp "$GF_PATHS_HOME/conf/sample.ini" "$GF_PATHS_CONFIG" &&\
       cp "$GF_PATHS_HOME/conf/ldap.toml" /etc/grafana/ldap.toml &&\
       chown -R grafana:grafana "$GF_PATHS_DATA" "$GF_PATHS_HOME/.aws" "$GF_PATHS_LOGS" "$GF_PATHS_PLUGINS" "$GF_PATHS_PROVISIONING" &&\
       chmod -R 777 "$GF_PATHS_DATA" "$GF_PATHS_HOME/.aws" "$GF_PATHS_LOGS" "$GF_PATHS_PLUGINS" "$GF_PATHS_PROVISIONING" &&\
       chmod +x /run.sh

EXPOSE 3000
USER grafana
ENTRYPOINT [ "/run.sh" ]
