# rok4 and sample dataset on docker base
#
# Fastcgi mode listening on port 9000
#
# VERSION 1.0.0 - based on dduportal initial work (https://github.com/dduportal/ddu-dockers)
#
# DOCKER-VERSION 0.6.6

FROM base

MAINTAINER Christophe Uberri <cuberri@gmail.com>

# ROK4 build options
ENV ROK4_VERSION 0.14.0
ENV ROK4_SRC_URL http://www.rok4.org/data/rok4-${ROK4_VERSION}.tar.bz2
ENV ROK4_DATASET_URL http://www.rok4.org/data/SCAN1000_JPG_LAMB93_FXX.zip
ENV ROK4_DATASET_MD5 344a8bbb6c1decfb3a8e171d4d20e6b4
ENV ROK4_INSTALL_DIR /usr/local/rok4/${ROK4_VERSION}

# Get a clean worker
RUN apt-get update && apt-get upgrade -y

# Install deps
RUN apt-get install -y build-essential curl gettext nasm automake cmake unzip

# Get rok4 sources and build it
RUN cd /tmp;curl ${ROK4_SRC_URL} | tar xjf -
RUN mv /tmp/$(basename $(cd /tmp/;find . -maxdepth 1 -type d -name "rok4*")) /tmp/rok4;mkdir -p /tmp/rok4/build
RUN cd /tmp/rok4/build;cmake .. -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX=${ROK4_INSTALL_DIR} -DCMAKE_BUILD_TYPE=Release -DBUILD_ROK4=TRUE -DBUILD_BE4=FALSE -DBUILD_DOC=FALSE -DUNITTEST=FALSE -DDEBUG_BUILD=FALSE && make && make install

# Configure rok4
RUN cp ${ROK4_INSTALL_DIR}/config/server.conf.sample ${ROK4_INSTALL_DIR}/config/server.conf
RUN cp ${ROK4_INSTALL_DIR}/config/services.conf.sample ${ROK4_INSTALL_DIR}/config/services.conf
RUN sed -i.bak 's#\.\./config#/usr/local/rok4/current/config#g' ${ROK4_INSTALL_DIR}/config/server.conf
RUN sed -i.bak 's#<serverPort></serverPort>#<serverPort>:9000</serverPort>#g' ${ROK4_INSTALL_DIR}/config/server.conf
RUN sed -i.bak 's#<logFilePrefix>/var/tmp/rok4</logFilePrefix>#<logFilePrefix>/var/log/rok4</logFilePrefix>#g' ${ROK4_INSTALL_DIR}/config/server.conf

# Make some links in order to manage other rok4 versions
RUN ln -s ${ROK4_INSTALL_DIR} /usr/local/rok4/current && ln -s ${ROK4_INSTALL_DIR}/config/server.conf /usr/local/rok4/server.conf

# Install sample dataset
RUN curl -o /tmp/SCAN1000_JPG_LAMB93_FXX.zip "${ROK4_DATASET_URL}" && [ \"$(md5sum /tmp/SCAN1000_JPG_LAMB93_FXX.zip | cut -d' ' -f1)\" = \"${ROK4_DATASET_MD5}\" ]
RUN mkdir -p /tmp/dataset && cd /tmp/dataset && unzip ../SCAN1000_JPG_LAMB93_FXX.zip
RUN sed -i 's/CHEMIN_A_REMPLACER/./g' /tmp/dataset/SCAN1000_JPG_LAMB93_FXX.lay
RUN cp -R /tmp/dataset/* /usr/local/rok4/current/config/layers

# Cleaning build dir
RUN apt-get autoremove -y --purge build-essential gettext nasm automake cmake curl unzip
RUN rm -rf /tmp/*

# Running ROK4
EXPOSE 9000

# configure container entrypoint and default options
CMD ["-f", "/usr/local/rok4/current/config/server.conf"]
ENTRYPOINT ["/usr/local/rok4/current/bin/rok4"]
