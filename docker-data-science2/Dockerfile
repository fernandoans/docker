# Base da Imagem
FROM openjdk:8-jre-slim

# Adicionar o metadata para a imagem com o par: chave,valor
LABEL maintainer="Fernando Anselmo <fernando.anselmo74@gmail.com>"
LABEL version="2.0"

# Variaveis de Ambiente
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8 PATH=/opt/conda/bin:$PATH

# Execucoes iniciais:
    # 1. Adicionar a Zona Geografica correta
RUN ln -fs /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime

    # 3. Instalar os pacotes necessarios
RUN mkdir -p /opt/workspace && \
    apt-get update -y && \
    apt-get install -y python3 && \
    ln -s /usr/bin/python3 /usr/bin/python && \
    rm -rf /var/lib/apt/lists/*

    # 4. Instalar os pacotes
RUN apt-get update -y && \
    apt-get install -y python3-pip && \
    pip3 install jupyterlab

RUN pip install scipy SQLAlchemy missingno json_tricks \
        gensim elasticsearch psycopg2-binary pymc3 apyori

    # 5. Instalar os temas para o Jupyter
RUN pip install msgpack jupyterthemes && \
    jt -t grade3

    # 6. Adicionar o Git ao Jupyter Lab
RUN pip install --upgrade jupyterlab-git

    # 7. Proceder limpeza final
RUN apt autoremove -y && \
    apt autoclean -y

# Configurar o acesso ao Jupyter
ENV SHARED_WORKSPACE=/opt/workspace

EXPOSE 8888
WORKDIR /root/GitProjects
CMD jupyter lab --ip=0.0.0.0 --port=8888 --no-browser --allow-root --NotebookApp.token='data-science'
