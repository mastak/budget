FROM ubuntu:17.10

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        graphviz \
        apt-utils \
        software-properties-common \
        curl \
        nano \
        unzip \
        openssh-server \
        python3 \
        python3-dev \
        python-distribute \
        python3-pip \
        git

RUN apt-get install -y --no-install-recommends \
        libboost-program-options-dev \
        zlib1g-dev \
        libboost-python-dev

RUN pip3 install --upgrade \
        pip \
        cython \
        setuptools \
        wheel

RUN pip3 install --upgrade \
        numpy \
        scipy \
        matplotlib \
        scikit-learn \
        pandas \
        seaborn \
        tensorflow \
        pystan \
        plotly

RUN pip3 install --upgrade \
        jupyter \
        statsmodels \
        nose \
        tqdm \
        pydot \
        watermark \
        geopy \
        joblib \
#        fbprophet \
        keras

RUN pip3 install notebook==5.6.0 qgrid cufflinks

RUN jupyter notebook --allow-root --generate-config -y
RUN echo "c.NotebookApp.password = ''" >> ~/.jupyter/jupyter_notebook_config.py
RUN echo "c.NotebookApp.token = ''" >> ~/.jupyter/jupyter_notebook_config.py
RUN jupyter nbextension enable --py --sys-prefix widgetsnbextension


COPY entrypoint.sh /

# Final setup: directories, permissions, ssh login, symlinks, etc
RUN mkdir -p /home/user && \
    mkdir -p /var/run/sshd && \
    echo 'root:12345' | chpasswd && \
    sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config && \
    sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

WORKDIR /notebooks
EXPOSE 22 4545

ENTRYPOINT ["/entrypoint.sh"]
CMD ["jupyter"]
