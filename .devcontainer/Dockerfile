FROM python:3.10-alpine
# 指定基础镜像为Python 3.10的alpine版本

ENV TZ Europe/Amsterdam
# 设置时区为欧洲阿姆斯特丹

ARG USERNAME=devcontainer
ARG USER_UID=1000
ARG USER_GID=$USER_UID
# 定义用户名和用户ID、组ID，可被后续命令引用

RUN apk --no-cache update && apk --no-cache upgrade && \
    apk add --no-cache gcc musl-dev linux-headers libffi-dev bash zsh git curl sudo build-base openssh-client
# 更新源并安装gcc、musl-dev、linux-headers、libffi-dev、bash、zsh、git、curl、sudo、build-base和openssh-client等软件包

RUN pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir wheel pre-commit
# 使用pip安装最新版pip，并安装wheel和pre-commit

RUN sed -i -e "s/bin\/ash/bin\/zsh/" /etc/passwd
# 修改默认shell为zsh

RUN addgroup -g $USER_GID $USERNAME \
    && adduser -D -u $USER_UID -G $USERNAME -s /bin/zsh $USERNAME
# 创建一个名为$USERNAME的用户和其对应的组，设置登录Shell为zsh

RUN echo "$USERNAME ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
# 允许$USERNAME用户使用sudo命令免密码

USER $USERNAME
# 切换至$USERNAME用户身份

RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
# 安装oh-my-zsh

RUN git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
# 安装zsh-autosuggestions插件

COPY config/zshrc /home/$USERNAME/.zshrc
# 复制自定义的zsh配置文件到$USERNAME用户目录下

COPY config/gitconfig /home/$USERNAME/.gitconfig
# 复制自定义的git配置文件到$USERNAME用户目录下
