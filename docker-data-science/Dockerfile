# Base da Imagem
FROM ubuntu:19.10

# Adiciona o metadata para a imagem com o par: chave,valor
LABEL maintainer="Fernando Anselmo <fernando.anselmo74@gmail.com>"
LABEL version="1.2"

# Variaveis de Ambiente
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8 PATH=/opt/conda/bin:$PATH

# Execucoes iniciais:
    # Cria a pasta de ligacao
RUN mkdir ~/GitProjects && \
    # instala os pacotes necessarios
    apt-get update && apt-get install --no-install-recommends --yes python3 && \
    apt-get install -y wget ca-certificates git-core pkg-config tree freetds-dev apt-utils && \
    # Limpeza basica
    apt-get autoclean -y && \ 
    rm -rf /var/lib/apt/lists/* && \
    # Configuracao do Jupyter
    mkdir ~/.ssh && touch ~/.ssh/known_hosts && \ 
    ssh-keygen -F github.com || ssh-keyscan github.com >> ~/.ssh/known_hosts && \
    git clone https://github.com/bobbywlindsey/dotfiles.git && \
    mkdir ~/.jupyter && \
    mkdir -p ~/.jupyter/custom && \
    mkdir -p ~/.jupyter/nbconfig && \
    cp /dotfiles/jupyter/jupyter_notebook_config.py ~/.jupyter/ && \
    cp /dotfiles/jupyter/custom/custom.js ~/.jupyter/custom/ && \
    cp /dotfiles/jupyter/nbconfig/notebook.json ~/.jupyter/nbconfig/ && \
    rm -rf /dotfiles && \
    # Instalar o Anaconda
    echo 'export PATH=/opt/conda/bin:$PATH' > /etc/profile.d/conda.sh && \ 
    wget --quiet https://repo.anaconda.com/archive/Anaconda3-2020.02-Linux-x86_64.sh -O ~/anaconda.sh && \
    /bin/bash ~/anaconda.sh -b -p /opt/conda && \
    rm ~/anaconda.sh && \
    conda uninstall anaconda-navigator && \
    conda update conda && \
    conda update anaconda && \
    conda install nodejs && \
    conda update --all && \
    # Limpeza basica no Anaconda
    find /opt/conda/ -follow -type f -name '*.a' -delete && \
    find /opt/conda/ -follow -type f -name '*.pyc' -delete && \
    find /opt/conda/ -follow -type f -name '*.js.map' -delete && \
    find /opt/conda/lib/python*/site-packages/bokeh/server/static -follow -type f -name '*.js' ! -name '*.min.js' -delete &&  \
    # Instalar os temas para o Jupyter
    pip install msgpack jupyterthemes && \ 
    jt -t grade3 && \
    # Instalar os pacotes 
    conda install scipy && \
    conda install pymssql mkl=2018 && \
    pip install SQLAlchemy missingno json_tricks \
        gensim elasticsearch psycopg2-binary \
        jupyter_contrib_nbextensions mysql-connector-python \
        jupyter_nbextensions_configurator pymc3 apyori && \
    # Habilitar as extensoes do Jupyter Notebook
    jupyter contrib nbextension install --user && \
    jupyter nbextensions_configurator enable --user && \
    jupyter nbextension enable codefolding/main && \
    jupyter nbextension enable collapsible_headings/main && \
    # Adicionar a extensao do vim-binding
    mkdir -p $(jupyter --data-dir)/nbextensions && \
    git clone https://github.com/lambdalisue/jupyter-vim-binding $(jupyter --data-dir)/nbextensions/vim_binding && \
    cd $(jupyter --data-dir)/nbextensions \
    chmod -R go-w vim_binding && \
    # Remover o que nao eh necessario
    conda clean -afy && \
    apt-get remove -y wget git-core pkg-config && \
    apt-get autoremove -y && apt-get autoclean -y && \
    # Adicionar o Git ao Jupyter Lab
    pip install --upgrade jupyterlab-git && \
    jupyter lab build

# Configurar o acesso ao Jupyter
WORKDIR /root/GitProjects
EXPOSE 8888
CMD jupyter lab --no-browser --ip=0.0.0.0 --allow-root --NotebookApp.token='data-science'
