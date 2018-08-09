FROM ecolell/flask-test-server:python3.6.6
MAINTAINER Eloy Adonis Colell <eloy.colell@gmail.com>

ARG DEBIAN_FRONTEND=noninteractive

# nvm environment variables
ENV ALGORITHMS_PACKAGES freecontact rabbitmq-server cmake
ENV PATH="${PATH}:/julia-ae26b25d43/bin:/CCMpred/bin"
# install apt
RUN apt-get update && apt-get install -y apt-utils postgresql
RUN apt-get install -y $ALGORITHMS_PACKAGES

# install ccmpred
RUN git clone --recursive https://github.com/soedinglab/CCMpred.git && \
    cd CCMpred && \
    cmake . && make

# update to julia 0.4
RUN wget https://julialang-s3.julialang.org/bin/linux/x64/0.4/julia-0.4.7-linux-x86_64.tar.gz && \
    tar -zxvf julia-0.4.7-linux-x86_64.tar.gz && \
    rm -rf julia-0.4.7-linux-x86_64.tar.gz
# install MIToS.js inside julia
RUN julia -e 'Pkg.clone("https://github.com/ecolell/GaussDCA.jl"); Pkg.update(); Pkg.add("MIToS"); Pkg.pin("MIToS", v"1.2.3"); Pkg.update()'


EXPOSE 4369 5555
