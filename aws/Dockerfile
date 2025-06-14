FROM ubuntu:22.04

# Evitar interações durante a instalação
ENV DEBIAN_FRONTEND=noninteractive

# Configurar mirror alternativo e adicionar opções para melhorar a resiliência
RUN echo 'Acquire::Retries "10";' > /etc/apt/apt.conf.d/80retries && \
    echo 'Acquire::http::Timeout "120";' > /etc/apt/apt.conf.d/99timeout && \
    echo 'Acquire::https::Timeout "120";' >> /etc/apt/apt.conf.d/99timeout

# Atualizar apt e instalar dependências com flags para melhor tolerância a falhas
#openssh-client incluido posteriormente para acesso de maquinas via ssh
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get update --option Acquire::Retries=10 && \
    apt-get install -y --no-install-recommends \
    # yum-utils \
    curl \
    wget \
    unzip \
    git \
    openssh-client \ 
    vim \
    nano \
    python3 \
    python3-pip \
    jq \
    less \
    groff \
    ca-certificates \
    gnupg \
    lsb-release \
    sudo \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Instalar Terraform
RUN wget --tries=5 --timeout=30 -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg \
    && echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | tee /etc/apt/sources.list.d/hashicorp.list \
    && apt-get update --option Acquire::Retries=10 && apt-get install -y terraform \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Instalar AWS CLI v2
RUN curl --retry 5 --retry-delay 5 "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" \
    && unzip awscliv2.zip \
    && ./aws/install \
    && rm -rf aws awscliv2.zip

# Criar um usuário não-root
RUN useradd -m -s /bin/bash developer \
    && echo "developer ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/developer \
    && chmod 0440 /etc/sudoers.d/developer

# Definir diretório de trabalho
WORKDIR /home/developer/project

# Mudar para o usuário developer
USER developer

# Manter o container rodando
CMD ["tail", "-f", "/dev/null"]