# This dockerfile is modified from https://github.com/tensorflow/tensorflow/blob/master/tensorflow/tools/docker/Dockerfile

FROM pdonorio/py3kbase

MAINTAINER Sungchul Choi <sc82.choi@gachon.ac.kr>

# Force bash always
RUN rm /bin/sh && ln -s /bin/bash /bin/sh
# Default miniconda installation
ENV CONDA_ENV_PATH /opt/conda
ENV MY_CONDA_PY3ENV "ml_python34"
# This is how you will activate this conda environment
ENV CONDA_ACTIVATE "source $CONDA_ENV_PATH/bin/activate $MY_CONDA_PY3ENV"

RUN wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh && \
    /bin/bash ~/miniconda.sh -b -p /opt/conda && \
    rm ~/miniconda.sh
ENV PATH $CONDA_ENV_PATH/bin:$PATH

###############################
# Extra libs
RUN apt-get install -y \
# Needed by matplotlib inline
    python-qt4 \
# Needed by howdoi
    libxml2-dev libxslt-dev openssh-server

WORKDIR /tmp

# Switch to python 3.5 very soon
RUN conda create -y -n $MY_CONDA_PY3ENV python=3.4
RUN conda update --quiet --yes conda

RUN conda install -y -n $MY_CONDA_PY3ENV \
        pip jupyter ipython notebook ipywidgets \
        six sqlalchemy cython pyzmq statsmodels \
        theano tornado jinja2 sphinx pygments nose readline openpyxl xlrd
RUN conda install -y -n $MY_CONDA_PY3ENV \
        numpy scipy pandas scikit-learn sympy \
        matplotlib seaborn bokeh

RUN conda install -y -n $MY_CONDA_PY3ENV \
        gensim

RUN conda install -y -c jjhelmus -n $MY_CONDA_PY3ENV \
    tensorflow=0.9.0

RUN conda clean -y -t

###############################
# Add other libraries not found in anaconda
RUN $CONDA_ACTIVATE && pip install howdoi
#RUN python -m ipykernel.kernelspec

# Note: this is how you use the conda pip instead of the existing one

###############################

# Copy sample notebooks.
CMD ["mkdir /notebook"]
# Jupyter has issues with being run directly:
#   https://github.com/ipython/ipython/issues/7062
# We just add a little wrapper script.
WORKDIR "/notebooks"

# TensorBoard
EXPOSE 6006
# IPython
EXPOSE 8888
# SSHD
EXPOSE 22


###############################
# SSHD for remote interpreter
RUN mkdir /var/run/sshd

RUN echo 'root:root' |chpasswd

RUN sed -ri 's/^PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config

###############################

# Script: Activate virtualenv and launch notebook
ENV STARTSCRIPT /opt/start
#RUN source activate $MY_CONDA_PY3ENV
RUN echo "#!/bin/bash" > $STARTSCRIPT
RUN echo "$CONDA_ACTIVATE" >> $STARTSCRIPT
RUN echo -e "# install kernels\npython -m ipykernel.kernelspec" >> $STARTSCRIPT
RUN echo -e "tensorboard --logdir=/logs --host 0.0.0.0 &" >> $STARTSCRIPT
RUN echo -e "/usr/sbin/sshd -D &" >> $STARTSCRIPT
RUN echo -e "# launch notebook\njupyter notebook --ip=0.0.0.0 --no-browser --matplotlib=inline" >> $STARTSCRIPT
RUN chmod +x $STARTSCRIPT
CMD ["/opt/start"]