FROM jeffreymanning/r-base

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
      io.openshift.expose-services="R-packages" \
      io.openshift.tags="centos7,R,java,maven"

USER default

# setup some environment stuff -
# https://cran.r-project.org
# https://cran.cnr.berkeley.edu/
# http://cran.stat.ucla.edu/
# http://lib.stat.cmu.edu/R/CRAN/
# https://cran.rstudio.com/
#ARG CRAN_REPO=https://cran.cnr.berkeley.edu/
#ARG BIO_REPO=https://bioconductor.org/

#RUN echo 'options(repos = c(CRAN = "${CRAN_REPO}"), download.file.method = "libcurl")' >> ${R_HOME}/etc/Rprofile.site

# miniml [packages required to build Spark R/knitr interpreter]
# need dev tools
RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('openssl'), lib=Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('withr'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('mime'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('curl'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('R6'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('whisker'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('jsonlite'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('digest'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('httr'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('git2r'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('rstudioapi'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('memoise'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('devtools'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"

#repl
RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('devtools'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "require('devtools'); install_github('IRkernel/repr')"

#
RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('base64enc'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('stringi'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('magrittr'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('stringr'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('evaluate'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('base64enc'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"

# knitr
RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('highr'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('markdown'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('yaml'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('knitr'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
##RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('libcurl-devel'), repos='${CRAN_REPO}')"
##RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('curl'), repos='${CRAN_REPO}')"
##RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('RCurl'), repos='${CRAN_REPO}')"
#
### the remaining packages are installed through R code
### install littler...  helps with scripting/installing
##RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('littler'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
##
###install docopt...  also helps with installation (very cool) - install stringr 1st...
##RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('docopt'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
##
### finally devtools to build out of git repos (if necessary)http support/curl (will be loaded by required)
##RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('curl'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
##RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('R6'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
##RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('whisker'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
##RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('httr'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#
## base for plotting
##RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('ggplot2'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
##RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('mplot'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
##RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('googleVis'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#
#
#repl
# RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('evaluate'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "install.packages(pkgs=c('devtools'), Sys.getenv('R_LIBS_USER'), repos='${CRAN_REPO}')"
#RUN /usr/bin/Rscript --slave --no-save --no-restore-history -e "require('devtools'); install_github('IRkernel/repr')"

#### Define default command.
# R causes backoff loop failures - default to centos7-base
#CMD ["R"]
#CMD R


