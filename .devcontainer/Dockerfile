# ベースとなるイメージ。Python 3系の安定版のスリム版。
FROM ubuntu:jammy

# ユーザ'vscode'を作成する
# 他のリポジトリではUSER_UIDがベースのイメージですでに使われている場合は別の値を使っています。
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# ユーザの作成と`sudo`の利用設定
# 言語・場所設定
RUN apt-get update \
    && groupadd --gid $USER_GID $USERNAME \
    && useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME \
    && apt-get -y install locales \
    && localedef -f UTF-8 -i ja_JP ja_JP.UTF-8 

# その他環境変数など
ENV LANG ja_JP.UTF-8
ENV LANGUAGE ja_JP:ja
ENV LC_ALL ja_JP.UTF-8
ENV TZ JST-9
ENV TERM xterm

# その他のツールのインストール
RUN apt-get -y install curl git apt ca-certificates locales openssh-client unzip make vim less gcc openjdk-17-jdk \
    && apt-get clean

RUN apt -y install maven \
    && apt-get clean
# ユーザの切り替え
USER $USERNAME
WORKDIR /home/$USERNAME/

# gitの補完設定
RUN cd /home/$USERNAME/ \
    && curl -o .git-completion.sh \
    https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash \
    && curl -o .git-prompt.sh \
    https://raw.githubusercontent.com/git/git/master/contrib/completion/git-prompt.sh
