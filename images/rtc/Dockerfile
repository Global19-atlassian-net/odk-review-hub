FROM jupyter/scipy-notebook:1386e2046833

ENV JUPYTERLAB_SRCDIR=$CONDA_DIR/src/jupyterlab
ENV JUPYTERLAB_REF=371ebbb4c3f15d3797b70a9721eb12faf84b0335

RUN mkdir -p $JUPYTERLAB_SRCDIR \
 && cd $JUPYTERLAB_SRCDIR \
 && git init \
 && git remote add origin https://github.com/minrk/jupyterlab \
 && git fetch --depth 1 origin \
    $JUPYTERLAB_REF \
 && git checkout FETCH_HEAD

RUN cd $JUPYTERLAB_SRCDIR \
 && jupyter lab clean \
 && pip install -v -e . \
 && jlpm install \
 && jlpm run build \
 && jupyter lab clean


RUN pip install --no-cache nbgitpuller

COPY jupyter_notebook_config.py /etc/jupyter/

USER root

ENV HOME=/home/user
RUN mkdir $HOME \
 && chown $NB_USER $HOME \
 && usermod -d $HOME $NB_USER
USER $NB_USER

WORKDIR $HOME

ADD entrypoint /usr/local/bin/entrypoint
ENTRYPOINT ["/usr/local/bin/entrypoint"]
