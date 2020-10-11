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
	&& apt-get install -y pdns-recursor dnsutils

# RUN echo 'deb [arch=amd64] http://repo.powerdns.com/ubuntu bionic-auth-43 main' \
#       >> /etc/apt/sources.list \
#  && echo 'Package: pdns-*' \
#       > /etc/apt/preferences.d/pdns \
#  && echo 'Pin: origin repo.powerdns.com' \
#       >> /etc/apt/preferences.d/pdns \
#  && echo 'Pin-Priority: 600' \
#       >> /etc/apt/preferences.d/pdns

# cleanup
RUN set -ex \
    && apt-get clean \
	&& rm -rf /var/lib/apt/lists/*

#RUN rm -rf /etc/powerdns/
#VOLUME [ "/etc/powerdns" ] 

RUN sed -i -e "s/# dnssec=process-no-validate/dnssec=validate/g" /etc/powerdns/recursor.conf

EXPOSE 53/tcp
EXPOSE 53/udp

ENV MAX_NEGATIVE_TTL=300
ENV NETWORKS=
ENV LOCAL_ADDRESS=
ENV QUERY_LOCAL_ADDRESS=
ENV SERVER_ID=


#COPY ./entrypoint.sh /

#ENTRYPOINT [ "/entrypoint.sh" ]

CMD [ "/bin/sh", "-c", "pdns_recursor --max-negative-ttl=${MAX_NEGATIVE_TTL} --allow-from=${NETWORKS} --local-address=${LOCAL_ADDRESS} --server-id=${SERVER_ID}" ]
