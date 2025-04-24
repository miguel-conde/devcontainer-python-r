# syntax=docker/dockerfile:1.4
FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive
ENV USERNAME=devuser \
    RSTUDIO_USER=rstudio \
    RSTUDIO_PASSWORD=rstudio \
    PATH=$PATH:/home/devuser/.local/bin

# --- Dependencias base
RUN apt-get update && apt-get install -y \
    sudo curl wget git zip unzip make build-essential cmake net-tools \
    software-properties-common libssl-dev libffi-dev \
    libcurl4-openssl-dev libxml2-dev locales zlib1g-dev \
    zsh gnupg lsb-release ca-certificates apt-transport-https \
    docker.io python3.12 python3.12-venv python3.12-dev \
    python3-pip r-base gdebi-core \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# --- pipx + pipenv + poetry
RUN apt-get update && apt-get install -y pipx && \
    pipx ensurepath && \
    pipx install pipenv && \
    pipx install poetry

# --- Crear usuario no-root (si no existe)
RUN if ! id -u $USERNAME > /dev/null 2>&1; then \
      useradd -m -s /bin/bash $USERNAME && \
      echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers; \
    fi

# --- Locale en_US.UTF-8
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# --- Instalar Quarto
RUN wget https://quarto.org/download/latest/quarto-linux-amd64.deb && \
    gdebi -n quarto-linux-amd64.deb && \
    rm quarto-linux-amd64.deb

# --- Instalar CmdStanR (como root, antes de cambiar de usuario)
RUN Rscript -e "install.packages('remotes', repos='https://cloud.r-project.org')" && \
    Rscript -e "remotes::install_github('stan-dev/cmdstanr')" && \
    Rscript -e "cmdstanr::install_cmdstan()"


# --- Parte opcional: RStudio Server (comentada porque no funciona en Ubuntu 24.04)
RUN wget https://s3.amazonaws.com/rstudio-ide-build/server/jammy/amd64/rstudio-server-2024.09.2-399-amd64.deb && \
    gdebi -n rstudio-server-2024.09.2-399-amd64.deb && \
    rm rstudio-server-2024.09.2-399-amd64.deb
RUN useradd -m $RSTUDIO_USER && echo "$RSTUDIO_USER:$RSTUDIO_PASSWORD" | chpasswd

# --- Crear script entrypoint directamente en el contenedor
RUN echo '#!/bin/bash\n'\
'echo "[ENTRYPOINT] Lanzando rserver como devuser"\n'\
'/usr/lib/rstudio-server/bin/rserver --server-user=devuser --server-daemonize=0 &\n'\
'exec "$@"' > /usr/local/bin/entrypoint.sh && chmod +x /usr/local/bin/entrypoint.sh
# RUN echo '#!/bin/bash\n'\
# 'echo "[ENTRYPOINT] Entrando en entrypoint.sh"\n'\
# 'which rserver || echo "[ENTRYPOINT] ❌ rserver no encontrado"\n'\
# 'ls -l /usr/lib/rstudio-server/bin/rserver\n'\
# '/usr/lib/rstudio-server/bin/rserver --server-daemonize=0 &\n'\
# 'echo "[ENTRYPOINT] 🎯 RStudio Server lanzado (en background)"\n'\
# 'exec "$@"' > /usr/local/bin/entrypoint.sh && chmod +x /usr/local/bin/entrypoint.sh

# --- Cambiar a devuser
USER $USERNAME

# --- Configuración final
WORKDIR /workspaces
EXPOSE 8787
EXPOSE 8888

ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
CMD ["bash"]


