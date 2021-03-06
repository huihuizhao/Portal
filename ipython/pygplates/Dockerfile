##
# Image for pygplates Jupyter server
#

FROM ubuntu:14.04

RUN apt-get update

# install dependencies for pygplates
RUN apt-get install -y libglew-dev
RUN apt-get install -y python2.7-dev
RUN apt-get install -y libboost-dev libboost-python-dev libboost-thread-dev libboost-program-options-dev libboost-test-dev libboost-system-dev
RUN apt-get install -y libqt4-dev
RUN apt-get install -y libgdal1-dev
RUN apt-get install -y libcgal-dev
RUN apt-get install -y libproj-dev
RUN apt-get install -y libqwt-dev
RUN apt-get install -y libxrender-dev libice-dev libsm-dev libfreetype6-dev libfontconfig1-dev

# install wget to enable the pygplates source code to be downloaded from sourceforge
RUN apt-get update
RUN apt-get install -y wget git

# use wget to get the correct pygplates package from sourceforge
RUN wget http://sourceforge.net/projects/gplates/files/pygplates/beta-revision-12/pygplates-ubuntu-trusty_1.5_1_amd64.deb

# use dpkg to install 
RUN dpkg -i pygplates-ubuntu-trusty_1.5_1_amd64.deb

# install all the python and ipython notebook requirements
RUN apt-get install -y gcc python-pip
RUN apt-get install -y python-numpy python-scipy python-matplotlib python-pandas python-sympy python-nose
RUN pip install jupyter
RUN pip install ipyparallel
RUN apt-get install -y python-mpltoolkits.basemap

# Install non-linear optimisation package, with options to import into python
#RUN wget http://ab-initio.mit.edu/nlopt/nlopt-2.4.2.tar.gz
#RUN tar -xvzf nlopt-2.4.2.tar.gz
#RUN cd nlopt-2.4.2 && ./configure --enable-shared && make && make install

# Set python path to find pygplates and nlopt

RUN git clone https://github.com/tonysyu/mpltools.git /mpltools
RUN cd /mpltools && python setup.py install

RUN export LD_LIBRARY_PATH=/usr/local/lib

RUN useradd -ms /bin/bash ipython 

ENV NB_USER ipython

USER root

WORKDIR /home/$NB_USER/work
COPY jupyter_notebook_config.py /home/$NB_USER/.jupyter/
RUN chown -R $NB_USER:root /home/$NB_USER/.jupyter

# Add Tini
ENV TINI_VERSION v0.9.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini
ENTRYPOINT ["/tini", "--"]

CMD ["/home/ipython/work/ipython_notebook.sh"]

Env PYTHONPATH ${PYTHONPATH}:/usr/lib:/usr/lib/pygplates/revision12/
ADD . /home/ipython/work/

RUN chmod +x /home/$NB_USER/work/ipython_notebook.sh
RUN chown -R $NB_USER:root /home/$NB_USER/work

USER ipython
