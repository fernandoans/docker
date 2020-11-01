# Base da Imagem
FROM ubuntu:20.04

# Adicionar o metadata para a imagem com o par: chave,valor
LABEL maintainer="Fernando Anselmo <fernando.anselmo74@gmail.com>"
LABEL version="1.3"

# Variaveis de Ambiente
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8 PATH=/opt/conda/bin:$PATH

# Execucoes iniciais:
    # 1. Criar a pasta de ligacao
RUN mkdir ~/GitProjects

    # 2. Instalar os pacotes necessarios
RUN apt-get update && apt-get install --no-install-recommends --yes python3 && \
    apt-get install -y wget ca-certificates git-core pkg-config tree freetds-dev apt-utils

    # 3. Proceder limpeza inicial
RUN apt-get autoclean -y && apt-get autoremove -y \ 
    rm -rf /var/lib/apt/lists/*

    # 4. Adicionar a Zona Geografica correta
RUN ln -fs /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime
    apt-get install -y tzdata
    dpkg-reconfigure --frontend noninteractive tzdata
    
    # 5. Configurar o ambiente para receber o Jupyter
RUN mkdir ~/.ssh && touch ~/.ssh/known_hosts && \ 
    ssh-keygen -F github.com || ssh-keyscan github.com >> ~/.ssh/known_hosts && \
    git clone https://github.com/bobbywlindsey/dotfiles.git && \
    mkdir ~/.jupyter && \
    mkdir -p ~/.jupyter/custom && \
    mkdir -p ~/.jupyter/nbconfig && \
    cp /dotfiles/jupyter/jupyter_notebook_config.py ~/.jupyter/ && \
    cp /dotfiles/jupyter/custom/custom.js ~/.jupyter/custom/ && \
    cp /dotfiles/jupyter/nbconfig/notebook.json ~/.jupyter/nbconfig/ && \
    rm -rf /dotfiles

    # 6. Instalar o Anaconda
RUN echo 'export PATH=/opt/conda/bin:$PATH' > /etc/profile.d/conda.sh && \ 
    wget --quiet https://repo.anaconda.com/archive/Anaconda3-2020.02-Linux-x86_64.sh -O ~/anaconda.sh && \
    /bin/bash ~/anaconda.sh -b -p /opt/conda && \
    rm ~/anaconda.sh && \
    conda uninstall anaconda-navigator && \
    conda update conda && \
    conda update anaconda && \
    conda install nodejs && \
    conda update --all

    # 7. Proceder limpeza basica no Anaconda
RUN find /opt/conda/ -follow -type f -name '*.a' -delete && \
    find /opt/conda/ -follow -type f -name '*.pyc' -delete && \
    find /opt/conda/ -follow -type f -name '*.js.map' -delete && \
    find /opt/conda/lib/python*/site-packages/bokeh/server/static -follow -type f -name '*.js' ! -name '*.min.js' -delete

    # 8. Instalar os temas para o Jupyter
RUN pip install msgpack jupyterthemes && \ 
    jt -t grade3

    # 9. Instalar os pacotes 
RUN conda install scipy && \
    conda install pymssql mkl=2018 && \
    pip install SQLAlchemy missingno json_tricks \
        gensim elasticsearch psycopg2-binary \
        jupyter_contrib_nbextensions mysql-connector-python \
        jupyter_nbextensions_configurator pymc3 apyori

    # 10. Habilitar as extensoes do Jupyter Notebook
RUN jupyter contrib nbextension install --user && \
    jupyter nbextensions_configurator enable --user && \
    jupyter nbextension enable codefolding/main && \
    jupyter nbextension enable collapsible_headings/main

    # 11. Adicionar a extensao do vim-binding
RUN mkdir -p $(jupyter --data-dir)/nbextensions && \
    git clone https://github.com/lambdalisue/jupyter-vim-binding $(jupyter --data-dir)/nbextensions/vim_binding && \
    cd $(jupyter --data-dir)/nbextensions \
    chmod -R go-w vim_binding

    # 12. Proceder limpeza final
RUN conda clean -afy && \
    apt-get remove -y wget git-core pkg-config && \
    apt-get autoremove -y && apt-get autoclean -y

    # 13. Adicionar o Git ao Jupyter Lab
RUN pip install --upgrade jupyterlab-git && \
    jupyter lab build

# Configurar o acesso ao Jupyter
WORKDIR /root/GitProjects
EXPOSE 8888
CMD jupyter lab --no-browser --ip=0.0.0.0 --allow-root --NotebookApp.token='data-science'
