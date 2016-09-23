FROM ubuntu:16.04
MAINTAINER Josep Arús-Pous <josep@joseparus.com>

# install dependencies (with python3 support)
RUN cd; apt update && apt upgrade -y && apt install -y git wget flex bison build-essential cmake \
  python3 python3-dev python3-pip sqlite3 swig libsqlite3-dev libboost-all-dev swig && \
  apt install -y software-properties-common && apt-add-repository -y ppa:webupd8team/java && apt-get update && \
  echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && \
  echo debconf shared/accepted-oracle-license-v1-1 seen true | debconf-set-selections && \
  apt install -y oracle-java8-installer &&\
  pip3 install numpy

# install rdkit
RUN git clone -b Release_2016_03_4 https://github.com/rdkit/rdkit /rdkit && cd rdkit && mkdir build &&\
  cd build && mkdir -p /rdkit/External/java_lib && wget -O /rdkit/External/java_lib/junit.jar \
    "http://search.maven.org/remotecontent?filepath=junit/junit/4.10/junit-4.10.jar" &&\
  export JAVA_HOME=/usr/lib/jvm/java-8-oracle && cmake .. \
  -DPYTHON_EXECUTABLE='/usr/bin/python3' \
  -DPYTHON_INCLUDE_DIR='/usr/include/python3.5m' \
  -DPYTHON_LIBRARY='/usr/lib/x86_64-linux-gnu/libpython3.5m.so' \
  -DPYTHON_NUMPY_INCLUDE_PATH='/usr/local/lib/python3.5/dist-packages/numpy/core/include' \
  -DRDK_BUILD_SWIG_WRAPPERS=ON &&\
  make && make install

# cleanup job
RUN mkdir -p /usr/local/rdkit/python && mv /rdkit/lib /usr/local/rdkit/lib &&\
  mv /rdkit/rdkit /usr/local/rdkit/python/rdkit && mv /rdkit/Code /usr/local/rdkit/include &&\
  mkdir /usr/local/rdkit/java && mv /usr/local/rdkit/include/JavaWrappers/gmwrapper/*.jar /usr/local/rdkit/java/ &&\
  mv /usr/local/rdkit/include/JavaWrappers/gmwrapper/*.so /usr/local/rdkit/lib &&\
  find /usr/local/rdkit/include -type f -not \( -name '*.h' -o -name '*.hpp' \) -delete &&\
  find /usr/local/rdkit/include -type d -empty -delete &&\
  rm -fr /rdkit

# set env related stuff
ENV RDBASE="/usr/local/rdkit"
ENV CLASSPATH="$CLASSPATH:$RDBASE/java/org.RDKit.jar"\
    LD_LIBRARY_PATH="$RDBASE/lib:$LD_LIBRARY_PATH"\
    PYTHONPATH="$RDBASE/python:$PYTHONPATH"

CMD ["python3", "-i"]
