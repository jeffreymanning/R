FROM jeffreymanning/sparkr-base

LABEL maintainer="JWM" \
      vendor="MITRE Corp" \
      version="1.0" \
      release="153" \
      summary="MITRE's base RStudio, Spark-R image - 1.0.53/3.4.x/2.1.1 API node" \
      description="Centos7, RStudio, Spark, R typically used for API nodes" \
### Required labels above - recommended below
      io.k8s.description="Centos, RStudio, Spark-R base API node image" \
      io.k8s.display-name="centos, RStudio, Spark-R API node image" \
      io.openshift.expose-services="RSTUDIO" \
      io.openshift.tags="centos7,RStudio,Spark,R,java,maven"

USER root

#install the basic packages, must install sudo - some downstream consumers cannot run as root
RUN yum clean all && \
    rm -rf /var/cache/yum && \
    yum -y update && \
    yum clean all -y

# https://unix.stackexchange.com/questions/9314/no-such-file-or-directory-etc-init-d-functions
# see comment #5 and follow links (https://github.com/knirbhay/scrapy-cluster/commit/19a15764ee566ebc84c49e2d958804cfd093d556#diff-1f04c545f788e34c88db8b137b973571R17)
RUN yum -y install psmisc initscripts && \
    yum clean all -y

ARG R_STUDIO_MAJOR_VERSION=1
ARG R_STUDIO_UPDATE_VERSION=0
ARG R_STUDIO_BUILD_NUMBER=153
ARG R_STUDIO_VERSION=${R_STUDIO_MAJOR_VERSION}.${R_STUDIO_UPDATE_VERSION}.${R_STUDIO_BUILD_NUMBER}
ARG R_STUDIO_DOWNLOAD_URL=https://download2.rstudio.org
ARG R_STUDIO_INSTALL_DIR=/tmp
#ARG R_STUDIO_HOME=${R_STUDIO_INSTALL_DIR}/rstudio

ARG R_STUDIO_PKG_NAME=rstudio-server-rhel-${R_STUDIO_VERSION}-x86_64

###  R lives in a special download!
###  Start of Installation

#### ---- Zeppelin Installation -----
WORKDIR ${R_STUDIO_INSTALL_DIR}

#### ---- (Deployment mode use) (Download from Internet -- Deployment) ----
RUN wget -c ${R_STUDIO_DOWNLOAD_URL}/${R_STUDIO_PKG_NAME}.rpm \
    && yum --nogpgcheck -y install ${R_STUDIO_PKG_NAME}.rpm \
    && rm -f ${R_STUDIO_PKG_NAME}.rpm \
    && yum clean all -y


### configure r-studio
#RUN echo '\n\
#    \n# Configure httr to perform out-of-band authentication if HTTR_LOCALHOST \
#    \n# is not set since a redirect to localhost may not work depending upon \
#    \n# where this Docker container is running. \
#    \nif(is.na(Sys.getenv("HTTR_LOCALHOST", unset=NA))) { \
#    \n  options(httr_oob_default = TRUE) \
#    \n}' >> ${R_HOME}/etc/Rprofile.site \
#    && echo "PATH=\"\${PATH}\"" >> ${R_HOME}/etc/Renviron \

# setup start and stopping of r-studio; iff S6 overlay is being used
#RUN mkdir -p /etc/services.d/rstudio \
#    && echo '#!/bin/bash \
#        \n exec /usr/lib/rstudio-server/bin/rserver --server-daemonize 0' \
#        > /etc/services.d/rstudio/run \
#    && echo '#!/bin/bash \
#        \n rstudio-server stop' \
#        > /etc/services.d/rstudio/finish

RUN useradd rstudio \
    && echo "rstudio:rstudio" | chpasswd \
	&& chown rstudio:rstudio /home/rstudio \
    && usermod -aG staff rstudio \
    && usermod -aG 0 rstudio

#ENV PATH=/usr/lib/rstudio-server/bin:$PATH

# fix the user(s)..  if command line told us...
COPY userconf.sh /etc/cont-init.d/userconf

## running with "-e ADD=shiny" adds shiny server
#COPY add_shiny.sh /etc/cont-init.d/add
EXPOSE 8787

# get off of root..
#
# No r-studio wants to run from root???/
#USER rstudio

CMD /usr/lib/rstudio-server/bin/rserver --server-daemonize 0

