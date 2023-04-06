FROM node:18

RUN apt-get update
# Install dependencies
RUN apt-get install -y wget zsh git
# Install oh-my-zsh
RUN wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh \
    && cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc

RUN npm install -g pnpm@8