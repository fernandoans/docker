FROM continuumio/anaconda3

# Adicionar o metadata para a imagem com o par: chave,valor
LABEL maintainer="Fernando Anselmo <fernando.anselmo74@gmail.com>"
LABEL version="1.0"

# Adicionar a Zona Geografica correta
RUN ln -fs /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime

RUN apt-get update && apt-get install -y libgtk2.0-dev tzdata && \
    rm -rf /var/lib/apt/lists/*

RUN /opt/conda/bin/conda update -n base -c defaults conda
RUN /opt/conda/bin/conda install python
RUN /opt/conda/bin/conda install anaconda-client
RUN /opt/conda/bin/conda install jupyter -y
RUN /opt/conda/bin/conda install --channel https://conda.anaconda.org/menpo opencv3 -y
RUN /opt/conda/bin/conda install numpy pandas scikit-learn matplotlib seaborn pyyaml h5py keras -y
RUN /opt/conda/bin/conda upgrade dask
RUN pip install tensorflow imutils

RUN ["mkdir", "notebooks"]
COPY conf/.jupyter /root/.jupyter
COPY run_jupyter.sh /

# Jupyter and Tensorboard ports
EXPOSE 8888 6006

# Store notebooks in this mounted directory
VOLUME /notebooks

CMD ["/run_jupyter.sh"]      

