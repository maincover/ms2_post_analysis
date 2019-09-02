FROM r-base

# Install some basic utilities
RUN apt-get update && apt-get install -y \
    curl \
    ca-certificates \
    sudo \
    git \
    bzip2 \
    libx11-6 \
    openssh-server \
    sssd \
    software-properties-common \
 && rm -rf /var/lib/apt/lists/*



RUN mkdir /var/run/sshd
EXPOSE 5000
# create a working directory
RUN mkdir /app
WORKDIR /app



# install miniconda
RUN curl -so /app/miniconda.sh https://repo.continuum.io/miniconda/Miniconda3-4.5.1-Linux-x86_64.sh \
 && chmod +x /app/miniconda.sh \
 && /app/miniconda.sh -b -p miniconda \
 && rm /app/miniconda.sh
ENV PATH=/app/miniconda/bin:$PATH
ENV CONDA_AUTO_UPDATE_CONDA=false

# create a python 3.6 environment
RUN /app/miniconda/bin/conda install conda-build \
 && /app/miniconda/bin/conda create -y --name py27 python=2.7 \
 && /app/miniconda/bin/conda clean -ya

ENV CONDA_DEFAULT_ENV=py27
ENV CONDA_PREFIX=/app/miniconda/envs/$CONDA_DEFAULT_ENV
ENV PATH=$CONDA_PREFIX/bin:$PATH

RUN conda install anaconda
COPY requirements.txt /app/requirements.txt
WORKDIR /app
RUN python2.7 -m pip install -r requirements.txt


RUN R -e "install.packages('survival',dependencies=TRUE, repos='http://cran.rstudio.com/')"

RUN R -e "install.packages('bshazard',dependencies=TRUE, repos='http://cran.rstudio.com/')"

RUN R -e "install.packages('flexsurv',dependencies=TRUE, repos='http://cran.rstudio.com/')"

RUN apt-get update && apt-get install -y screen

RUN pip install dill
# Set default command to python3
CMD ["/bin/bash"]