FROM andersen9419/nvdrivercuda118:latest

ARG USER_ID="10000"
ARG GROUP_ID="10001"
ARG USER_NAME="user"

RUN groupadd -g "${GROUP_ID}" "${USER_NAME}" && \
  useradd -l -u "${USER_ID}" -m "${USER_NAME}" -g "${USER_NAME}"

# 環境変数を設定
ENV PYENV_ROOT /root/.pyenv
ENV PATH $PYENV_ROOT/shims:$PYENV_ROOT/bin:$PATH

# 必要なパッケージをインストール
RUN apt-get update && apt-get install -y \
    git \
    wget \
    build-essential \
    libffi-dev \
    libssl-dev \
    zlib1g-dev \
    liblzma-dev \
    libbz2-dev \
    libreadline-dev \
    libsqlite3-dev \
    llvm \
    libncurses5-dev \
    libncursesw5-dev \
    xz-utils \
    tk-dev \
    libffi-dev \
    liblzma-dev \
    python3-openssl \
    curl

# pyenvをインストール
RUN curl https://pyenv.run | bash

# Python 3.10.11のインストール
RUN pyenv install 3.10.11

# 仮想環境の設定
RUN pyenv virtualenv 3.10.11 myenv

# 仮想環境をグローバルに設定
RUN pyenv global myenv




# PyTorch, Jupyter Lab, ipykernel, Node.jsをインストール
RUN pip install torch==2.0.1 torchvision==0.15.2 torchaudio==2.0.2 --index-url https://download.pytorch.org/whl/cu118
RUN pip install jupyter
RUN pip install ipykernel
RUN pip install colorama
RUN apt-get install -y nodejs npm

# Jupyter Lab用のカーネルを設定
RUN python -m ipykernel install --user --name="myenv"

# 作業ディレクトリを設定
WORKDIR /workspace

COPY PaperSD.ipynb /workspace/



# Jupyter notebookを起動
EXPOSE 8888
CMD ["jupyter", "notebook", "--ip=0.0.0.0", "--port=8888", "--allow-root", "--NotebookApp.token=''"]
