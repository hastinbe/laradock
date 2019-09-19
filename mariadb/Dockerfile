ARG MARIADB_VERSION=latest
FROM mariadb:${MARIADB_VERSION}

LABEL maintainer="Mahmoud Zalt <mahmoud@zalt.me>"

#####################################
# Set Timezone
#####################################

ARG TZ=UTC
ENV TZ ${TZ}
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone && chown -R mysql:root /var/lib/mysql/

COPY my.cnf /etc/mysql/conf.d/my.cnf

# See https://click.palletsprojects.com/en/5.x/python3/
ENV LC_ALL C.UTF-8
ENV LANG en_US.UTF-8

###########################################################################
# mycli:
###########################################################################

ARG INSTALL_MYCLI=false

RUN if [ ${INSTALL_MYCLI} = true ]; then \
    set -xe; \
    apt-get update -yqq && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
          apt-utils \
          python3 \
          python3-pip \
          python3-setuptools \
          && \
      python3 -m pip install --upgrade pip && \
      python3 -m pip --quiet --no-cache-dir install \
          'six>=1.9.0' \
          mycli \
          && \
      ln -sf /usr/bin/python3 /usr/bin/python \
  ;fi \
;fi

#
#--------------------------------------------------------------------------
# Final Touch
#--------------------------------------------------------------------------
#

USER root

# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog

CMD ["mysqld"]

EXPOSE 3306
