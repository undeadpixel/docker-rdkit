FROM ubuntu:16.04

# install dependencies (with python3 support)
RUN apt update && apt upgrade -y && apt install -y git wget flex bison build-essential cmake python3 python3-dev python3-pip sqlite3 \
  libsqlite3-dev libboost-all-dev swig && \
  pip3 install numpy

# install rdkit
RUN git clone -b Release_2016_03_3 https://github.com/rdkit/rdkit && cd rdkit &&\
  mkdir build && cd build && cmake .. \
  -DPYTHON_EXECUTABLE='/usr/bin/python3' \
  -DPYTHON_INCLUDE_DIR='/usr/include/python3.5m' \
  -DPYTHON_LIBRARY='/usr/lib/x86_64-linux-gnu/libpython3.5m.so' \
  -DPYTHON_NUMPY_INCLUDE_PATH='/usr/local/lib/python3.5/dist-packages/numpy/core/include' &&\
  make && make install && mkdir -p /usr/local/rdkit/python && mv /rdkit/lib /usr/local/rdkit/lib &&\
  mv /rdkit/rdkit /usr/local/rdkit/python/rdkit && mv /rdkit/Code /usr/local/rdkit/include &&\
  find /usr/local/rdkit/include -type f -not \( -name '*.h' -o -name '*.hpp' \) -exec rm {} \; &&\
  rm -fr /rdkit

# set env related stuff
ENV RDBASE="/usr/local/rdkit"
ENV LD_LIBRARY_PATH="$RDBASE/lib:$LD_LIBRARY_PATH"\
    PYTHONPATH="$RDBASE/python:$PYTHONPATH"

CMD ["python3", "-i"]
