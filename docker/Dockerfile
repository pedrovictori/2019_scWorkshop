FROM rocker/rstudio:latest
MAINTAINER kgosik@broadinstitute.org

#########
### Install Utilities
#########
RUN apt-get update && apt-get install -y gcc g++ perl python python3 automake make cmake less vim \
                                       wget git curl bsdtar libdb-dev libglu1-mesa-dev \
                                       zlib1g-dev bzip2 libncurses5-dev \
                                       texlive-latex-base \
                                       default-jre \
                                       python-pip python3-pip python-dev \
                                       gfortran \
                                       build-essential libghc-zlib-dev libncurses-dev libbz2-dev liblzma-dev libpcre3-dev libxml2-dev \
                                       libblas-dev gfortran git unzip ftp libzmq3-dev nano ftp fort77 libreadline-dev \
                                       libcurl4-openssl-dev libx11-dev libxt-dev \
                                       x11-common libcairo2-dev libpng-dev libreadline6-dev libjpeg-dev pkg-config \
                                       build-essential cmake gsl-bin libgsl0-dev libeigen3-dev libboost-all-dev \
                                       libssl-dev libssl-dev libcairo2-dev libxt-dev libgtk2.0-dev libcairo2-dev xvfb xauth xfonts-base \
                                       apt-transport-https libhdf5-serial-dev

RUN cp $(which tar) $(which tar)~ \
    && ln -sf $(which bsdtar) $(which tar)

## Install R packages
COPY install.R /usr/local/src
## install necessary R packages
RUN Rscript /usr/local/src/install.R

## Install Python Requirements
COPY requirements.txt /usr/local/src
RUN pip install -r /usr/local/src/requirements.txt
RUN pip3 install -r /usr/local/src/requirements.txt

## FASTQC
COPY fastqc_v0.11.8.zip /usr/local/src/fastqc_v0.11.8.zip
RUN cd /usr/local/src/ && \
  unzip fastqc_v0.11.8.zip && \
  cd /


## IGViewer
COPY IGV_2.4.19.zip /usr/local/src/IGV_2.4.19.zip
RUN cd /usr/local/src && \
   unzip IGV_2.4.19.zip && \
   cd /


## cellranger
COPY cellranger-3.0.2.tar.gz /usr/local/src
RUN cd /usr/local/src && \
  tar -xvzf cellranger-3.0.2.tar.gz && \
  cd /

COPY cellranger_count_3.0.2.sh /usr/local/src


## Alternatively, get STAR source using git
RUN git clone https://github.com/alexdobin/STAR.git usr/local/src/STAR && \
  cd /usr/local/src/STAR/source && \
  make STAR && \
  cd /


RUN wget -O /usr/local/src/samtools-1.9.tar.bz2 https://github.com/samtools/samtools/releases/download/1.9/samtools-1.9.tar.bz2 && \
  cd /usr/local/src && \
  tar xvjf samtools-1.9.tar.bz2 && \
  cd samtools-1.9 && \
   ./configure  && \
   make && make install
