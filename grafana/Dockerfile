FROM grafana/grafana:4.4.2

RUN apt-get update && apt-get install -y curl gettext-base telnet supervisor && rm -rf /var/lib/apt/lists/*

WORKDIR /etc/grafana
COPY datasources ./datasources

WORKDIR /etc/grafana
COPY grafana.ini ./grafana.ini

RUN mkdir -p mkdir /opt/template/logs
ADD https://releases.hashicorp.com/consul-template/0.19.5/consul-template_0.19.5_linux_amd64.tgz /tmp/consul-template.tgz
RUN cd /tmp && tar xvfz /tmp/consul-template.tgz && mv consul-template /bin/consul-template 

WORKDIR /etc/grafana
COPY grafana.ini.ctmpl ./grafana.ini.ctmpl

WORKDIR /etc/grafana
COPY config.hcl ./config.hcl
COPY start.sh ./start.sh
RUN chmod u+x start.sh
RUN mkdir -p /etc/supervisor/conf.d/
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

WORKDIR /app
COPY entrypoint.sh ./
RUN chmod u+x entrypoint.sh

ENTRYPOINT ["/usr/bin/supervisord"]


