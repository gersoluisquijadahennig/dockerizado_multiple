# Node workspace Dockerfile (development)
# Creates a user matching host UID/GID, installs common dev tools and NVM,
# then installs the requested Node version for the workspace user.

FROM debian:bookworm-slim

# Build arguments
ARG UID=1000
ARG GID=1000
ARG NODE_VERSION=22.0.0
ARG NVM_VERSION=v0.39.4

ENV DEBIAN_FRONTEND=noninteractive
ENV NVM_DIR=/home/workspace/.nvm

# Install required packages
RUN apt-get update && apt-get install -y --no-install-recommends \
		ca-certificates \
		curl \
		wget \
		git \
		sudo \
		build-essential \
		ca-certificates \
		ca-certificates-java \
		gnupg \
		lsb-release \
		procps \
		bash \
 && rm -rf /var/lib/apt/lists/*

# Create group/user matching host IDs (reuse if exist)
RUN if getent group ${GID} >/dev/null 2>&1; then \
			GROUP_NAME=$(getent group ${GID} | cut -d: -f1); \
		else \
			GROUP_NAME=workspace && groupadd -g ${GID} ${GROUP_NAME}; \
		fi && \
		if getent passwd ${UID} >/dev/null 2>&1; then \
			USER_NAME=$(getent passwd ${UID} | cut -d: -f1); \
		else \
			USER_NAME=workspace && useradd -m -u ${UID} -g ${GROUP_NAME} -s /bin/bash ${USER_NAME}; \
		fi && \
		usermod -aG sudo ${USER_NAME} || true && echo "${USER_NAME} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# Install NVM and Node.js for the workspace user
RUN set -eux; \
		mkdir -p ${NVM_DIR} && chown -R ${UID}:${GID} /home/workspace; \
		su - workspace -c "curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/${NVM_VERSION}/install.sh | bash"; \
		su - workspace -c "export NVM_DIR=\"${NVM_DIR}\" && . \"${NVM_DIR}/nvm.sh\" && nvm install ${NODE_VERSION} && nvm alias default ${NODE_VERSION}"

# Ensure nvm is available in interactive shells
RUN echo 'export NVM_DIR="/home/workspace/.nvm"' >> /home/workspace/.bashrc && \
		echo '[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"' >> /home/workspace/.bashrc && \
		chown workspace:workspace /home/workspace/.bashrc

# Create a global script to load nvm for non-interactive shells
RUN cat > /usr/local/bin/load-nvm.sh <<'EOF'
#!/bin/bash
export NVM_DIR="/home/workspace/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
export PATH="$NVM_DIR/versions/node/v22.0.0/bin:$PATH"
EOF

RUN chmod +x /usr/local/bin/load-nvm.sh

# Create wrapper scripts for node and npm that work for all users
RUN ln -s /home/workspace/.nvm/versions/node/v22.0.0/bin/node /usr/local/bin/node && \
    ln -s /home/workspace/.nvm/versions/node/v22.0.0/bin/npm /usr/local/bin/npm && \
    ln -s /home/workspace/.nvm/versions/node/v22.0.0/bin/npx /usr/local/bin/npx

# Working directory
WORKDIR /var/www

# Switch to workspace user by default
USER workspace

# Default to an interactive shell
CMD ["bash", "-l"]
