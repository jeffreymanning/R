FROM jeffreymanning/centos7-base

MAINTAINER Jeff Manning
LABEL name="r-base" \
      vendor="MITRE Corp" \
      version="3.4" \
      release="0" \
      summary="MITRE's base R image - 3.4.x" \
      description="Centos7, R" \
### Required labels above - recommended below
      run='docker run -tdi --name ${NAME} ${IMAGE}' \
      io.k8s.description="Centos, R base image" \
      io.k8s.display-name="centos, R" \
      io.openshift.expose-services="R" \
      io.openshift.tags="centos7,R,java,maven"

USER root

# to run
# XSOCK=/tmp/.X11-unix && XAUTH=/tmp/.docker.xauth && xauth nlist ${DISPLAY} | sed -e "s/^..../ffff/" | xauth -f $XAUTH nmerge - && docker run  -v $XSOCK:$XSOCK -v $XAUTH:$XAUTH -e XAUTHORITY=$XAUTH  -e DISPLAY=$DISPLAY --rm -it jwmanning/r-base R -e "capabilities()"
# XSOCK=/tmp/.X11-unix && XAUTH=/tmp/.docker.xauth && touch $XAUTH && xauth nlist ${DISPLAY} | sed -e "s/^..../ffff/" | xauth -f $XAUTH nmerge - && docker run  -v $XSOCK -v $XAUTH -e XAUTHORITY=$XAUTH  -e DISPLAY=$DISPLAY --rm -it jwmanning/r-base R -e "capabilities()"


#install epel-release, cannot be combined
RUN yum clean all && \
    rm -rf /var/cache/yum && \
    yum -y update && \
#    yum -y install yum-utils && \
    yum -y install deltarpm && \
# epel-release
    yum -y install epel-release && \
# X11
#    INSTALL_X11_PKGS="libX11-devel libXt-devel libXmu-devel" && \
#    yum install -y --setopt=tsflags=nodocs ${INSTALL_X11_PKGS} && \
# various libraries
#    INSTALL_LIBS_PKGS="readline-devel pcre-devel libjpeg-devel libpng-devel libtiff-devel lzma-devel cairo-devel" && \
#    yum install -y --setopt=tsflags=nodocs ${INSTALL_LIBS_PKGS} && \
# openssl -  moved to centos7-base
#    INSTALL_SYSTEM_PKGS="openssl openssl-devel libcurl libcurl-devel" && \
#    yum install -y --setopt=tsflags=nodocs ${INSTALL_SYSTEM_PKGS} && \
# pre-reqs
#    INSTALL_PREREQ_PKGS="libicu libicu-devel blas blas-devel lapack lapack-devel libxml2 libxml2-devel texinfo texinfo-tex texlive-epsf \
#                   libcurl libcurl-devel \
#                   gnutls gnutls-devel " && \
    INSTALL_PREREQ_PKGS="libicu libicu-devel blas blas-devel lapack lapack-devel libxml2 libxml2-devel texinfo texinfo-tex texlive-epsf" && \
    yum install -y --setopt=tsflags=nodocs ${INSTALL_PREREQ_PKGS} && \
# https://support.rstudio.com/hc/en-us/articles/218004217-Building-R-from-source
# r install environment (devel/rt pre-reqs)
#    INSTALL_DEVEL_PKGS=" autoconf automake bzip2-devel cairo-devel gcc-c++ gcc-gfortran gcc-objc java-1.8.0-openjdk-headless \
#                        libICE-devel libSM-devel libX11-devel libXmu-devel libXt-devel \
#                        libcurl-devel libicu-devel libjpeg-turbo-devel libpng-devel libtiff-devel libtool ncurses-devel \
#                        openblas-devel pango-devel pcre-devel readline-devel tcl-devel \
#                        texinfo-tex texlive-collection-latexrecommended tk-devel tre-devel valgrind-devel xz-devel zlib-devel" && \
#    INSTALL_RUNTIME_PKGS="libtool \
#                        libX11 libICE libSM libXmu libXt \
#                        libicu libpng libtiff libjpeg-turbo \
#                        cairo pango pcre tcl tk \
#                        bzip2 libcurl zlib xz libxml2 \
#                        openblas \
#                        texinfo texinfo-tex texlive-epsf" && \
#    yum install -y --setopt=tsflags=nodocs ${INSTALL_RUNTIME_PKGS} && \
    INSTALL_R_PKGS="R R-core R-core-devel R-devel" && \
    yum install -y --setopt=tsflags=nodocs ${INSTALL_R_PKGS} && \
    yum clean all -y && \
    rm -rf /var/cache/yum

## alternatively.. using centos
# http://mirror.centos.org/centos/7/os/x86_64/Packages/<package>
# centos/7 is the latest version... but requires managing the version stamps
#RUN yum -y install http://mirror.centos.org/centos/7/os/x86_64/Packages/texlive-epsf-svn21461.2.7.4-38.el7.noarch.rpm
#RUN yum -y install http://mirror.centos.org/centos/7/os/x86_64/Packages/texinfo-5.1-4.el7.x86_64.rpm disable_gpg_check=yes state=present
#RUN yum -y install http://mirror.centos.org/centos/7/os/x86_64/Packages/texinfo-tex-5.1-4.el7.x86_64.rpm disable_gpg_check=yes state=present
#RUN yum -y install http://mirror.centos.org/centos/7/os/x86_64/Packages/blas-3.4.2-5.el7.x86_64.rpm disable_gpg_check=yes state=present
#RUN yum -y install http://mirror.centos.org/centos/7/os/x86_64/Packages/blas-devel-3.4.2-5.el7.x86_64.rpm disable_gpg_check=yes state=present
#RUN yum -y install http://mirror.centos.org/centos/7/os/x86_64/Packages/lapack-3.4.2-5.el7.x86_64.rpm disable_gpg_check=yes state=present
#RUN yum -y install http://mirror.centos.org/centos/7/os/x86_64/Packages/lapack-devel-3.4.2-5.el7.x86_64.rpm disable_gpg_check=yes state=present

## configure
#RUN mkdir -p /etc/R && \
#    touch /etc/R/Rprofile.site && \
#    touch /etc/littler.r && \
#    echo 'options(repos = c(CRAN = "https://cran.rstudio.com/"), download.file.method = "libcurl")' >> /etc/R/Rprofile.site
#    echo 'source("/etc/R/Rprofile.site")' >> /etc/littler.r && \
#    echo ln -s /usr/share/doc/littler/examples/install.r /usr/local/bin/install.r && \
#    ln -s /usr/share/doc/littler/examples/install2.r /usr/local/bin/install2.r  && \
#    ln -s /usr/share/doc/littler/examples/installGithub.r /usr/local/bin/installGithub.r && \
#    ln -s /usr/share/doc/littler/examples/testInstalled.r /usr/local/bin/testInstalled.r

### R is installed into /usr/local/lib/R
## Set a default user. Available via runtime flag `--user docker`
## Add user to 'staff' group, granting them write privileges to /usr/local/lib/R/site.library
## User should also have & own a home directory (for rstudio or linked volumes to work properly).

#RUN groupadd staff && \
##RUN useradd docker \
##	&& chown docker:docker /home/docker \
#    usermod -aG staff ${USER_NAME}
##    echo "${USER_NAME} ALL=(ALL)  ALL" >> /etc/sudoers

## Add a library directory (for user-installed packages)

# default package insatalltion to docker location...  will be lost if shutown
ARG R_STUDIO_LIB_SITE=/usr/local/lib/R/site-library
RUN mkdir -p ${R_STUDIO_LIB_SITE} && \
#    chown default:staff ${R_STUDIO_LIB_SITE} && \
    chown default:root ${R_STUDIO_LIB_SITE} && \
    chmod g+wx ${R_STUDIO_LIB_SITE}

## Fix library path to confirm (R then .libPaths())
# "R_LIBS=\${R_LIBS-'/usr/local/lib/R/site-library:/usr/share/R/library:/usr/lib64/R/library'}"
ENV R_HOME=/usr/lib64/R
ENV R_LIBS_USER=${R_STUDIO_LIB_SITE}
ENV R_LIBS=${R_LIBS_USER}:/usr/share/R/library:/usr/lib64/R/library

# setup some environment stuff -
# https://cran.r-project.org
# https://cran.cnr.berkeley.edu/
# http://cran.stat.ucla.a295d9ab6608du/
# http://lib.stat.cmu.edu/R/CRAN/
# https://cran.rstudio.com/
ENV CRAN_REPO=https://cran.cnr.berkeley.edu/
ENV BIO_REPO=https://bioconductor.org/

RUN echo 'options(repos = c(CRAN = "${CRAN_REPO}"), download.file.method = "libcurl")' >> ${R_HOME}/etc/Rprofile.site
#RUN echo "R_LIBS=\${R_LIBS-'/usr/local/lib/R/site-library:/usr/local/lib/R/library:/usr/lib/R/library'}" >> ${R_HOME}/etc/Renviron

# miniml [packages required to build Spark R/knitr interpreter]
# need dev tools
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('openssl'), lib=Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('mime'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('curl'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('R6'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('whisker'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('jsonlite'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('httr'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('git2r'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('memoise'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('rstudioapi'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('withr'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('digest'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('devtools'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "library('devtools'); devtools::install_github('IRkernel/repr')"

#
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('evaluate'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('base64enc'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('knitr'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('libcurl-devel'), repos='${CRAN_REPO}')"
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('curl'), repos='${CRAN_REPO}')"
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('RCurl'), repos='${CRAN_REPO}')"


## the remaining packages are installed through R code
## install littler...  helps with scripting/installing
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('littler'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#
##install docopt...  also helps with installation (very cool) - install stringr 1st...
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('docopt'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#
## finally devtools to build out of git repos (if necessary)http support/curl (will be loaded by required)
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('curl'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('R6'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('whisker'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('httr'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"

# base for plotting
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('ggplot2'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('mplot'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('googleVis'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"

# get off root, move to default
USER ${USER_NAME}
#USER root

#### Define default command.
# R causes backoff loop failures - default to centos7-base
#CMD ["R"]
#CMD R


