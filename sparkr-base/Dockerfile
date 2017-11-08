#FROM jeffreymanning/centos-base
# this is geared towards spark environment
FROM jeffreymanning/spark-base
LABEL maintainer="JWM" \
      vendor="MITRE Corp" \
      version="3.4" \
      release="0" \
      summary="MITRE's base Spark-R image - 3.4.x/2.1.1 API node" \
      description="Centos7, Spark, R typically used for API nodes" \
### Required labels above - recommended below
      io.k8s.description="Centos, Spark-R base API node image" \
      io.k8s.display-name="centos, Spark-R API node image" \
      io.openshift.expose-services="SparkR-base" \
      io.openshift.tags="centos7,Spark,R,java,maven"

USER root

RUN yum clean all && \
    rm -rf /var/cache/yum && \
    yum -y update && \
    yum -y install deltarpm && \
# epel-release
    yum -y install epel-release && \
    INSTALL_PREREQ_PKGS="libicu libicu-devel blas blas-devel lapack lapack-devel libxml2 libxml2-devel texinfo texinfo-tex texlive-epsf" && \
    yum install -y --setopt=tsflags=nodocs ${INSTALL_PREREQ_PKGS} && \
    INSTALL_R_PKGS="R R-core R-core-devel R-devel" && \
    yum install -y --setopt=tsflags=nodocs ${INSTALL_R_PKGS} && \
    yum clean all -y && \
    rm -rf /var/cache/yum

### R is installed into /usr/local/lib/R
## Set a default user. Available via runtime flag `--user docker`
## Add user to 'staff' group, granting them write privileges to /usr/local/lib/R/site.library
## User should also have & own a home directory (for rstudio or linked volumes to work properly).
RUN groupadd staff && \
    usermod -aG staff default

## Add a library directory (for user-installed packages)

# default package insatalltion to docker location...  will be lost if shutown
ARG R_STUDIO_LIB_SITE=/usr/local/lib/R/site-library
RUN mkdir -p ${R_STUDIO_LIB_SITE} && \
    chown default:staff ${R_STUDIO_LIB_SITE} && \
    chmod g+wx ${R_STUDIO_LIB_SITE}

## Fix library path to confirm (R then .libPaths())
# "R_LIBS=\${R_LIBS-'/usr/local/lib/R/site-library:/usr/share/R/library:/usr/lib64/R/library'}"
ENV R_HOME=/usr/lib64/R
ENV R_LIBS_USER=${R_STUDIO_LIB_SITE}
ENV R_LIBS=${R_LIBS_USER}:/usr/share/R/library:/usr/lib64/R/library

# setup some environment stuff -
ARG CRAN_REPO=https://cran.rstudio.com/
ARG BIO_REPO=https://bioconductor.org/

RUN echo 'options(repos = c(CRAN = "${CRAN_REPO}"), download.file.method = "libcurl")' >> ${R_HOME}/etc/Rprofile.site

# get off root, move to default
USER default

#### Define default command.
# R causes backoff loop failures - default to centos7-base
#CMD ["R"]


