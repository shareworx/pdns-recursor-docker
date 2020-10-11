FROM ubuntu:20.04

RUN set -ex ln -fs /usr/share/zoneinfo/Europ/Berlin /etc/localtime

RUN set -ex \
    && export DEBIAN_FRONTEND=noninteractive \
	&& apt-get update \
	&& apt-get install -y --no-install-recommends tzdata

RUN set -ex \
    dpkg-reconfigure --frontend noninteractive tzdata

RUN set -ex \
	# Prepare for adding repository
#	&& apt-get update \
	&& apt-get install -y dnsdist dnsutils

# RUN echo 'deb [arch=amd64] http://repo.powerdns.com/ubuntu bionic-auth-43 main' \
#       >> /etc/apt/sources.list \
#  && echo 'Package: pdns-*' \
#       > /etc/apt/preferences.d/pdns \
#  && echo 'Pin: origin repo.powerdns.com' \
#       >> /etc/apt/preferences.d/pdns \
#  && echo 'Pin-Priority: 600' \
#       >> /etc/apt/preferences.d/pdns

# cleanup
#RUN set -ex \
#    && apt-get clean \
#	&& rm -rf /var/lib/apt/lists/*

#RUN rm -rf /etc/powerdns/
#VOLUME [ "/etc/powerdns" ] 

EXPOSE 53/tcp
EXPOSE 53/udp
EXPOSE 853/tcp

COPY ./dnsdist.conf /etc/
COPY ./entrypoint.sh /

#ENTRYPOINT [ "/entrypoint.sh" ]

CMD [ "/bin/sh", "-c", "dnsdist -C /etc/dnsdist.conf --local=0.0.0.0:53" ]
