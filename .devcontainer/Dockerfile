### Based on microsoft's debian-based miniconda image
FROM mcr.microsoft.com/devcontainers/miniconda:0-3



### enable x11 forwarding based on
# https://github.com/microsoft/vscode-dev-containers/blob/main/repository-containers/images/github.com/microsoft/vscode/.devcontainer/base.Dockerfile
COPY .devcontainer/library-scripts/desktop-lite-debian.sh /tmp/library-scripts/
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
	&& bash /tmp/library-scripts/desktop-lite-debian.sh \
	&& apt-get -y install firefox-esr \
	&& echo 'export DBUS_SESSION_BUS_ADDRESS=$(grep -z DBUS_SESSION_BUS_ADDRESS /proc/$(pidof fluxbox)/environ|cut -d= -f2-)' | tee -a /etc/bash.bashrc > /etc/zsh/zshenv \
	&& apt-get clean -y && rm -rf /var/lib/apt/lists/* /tmp/library-scripts/
# Core environment variables for X11, VNC, and fluxbox
ENV DBUS_SESSION_BUS_ADDRESS="autolaunch:" \
	VNC_RESOLUTION="1440x768x16" \
	VNC_DPI="96" \
	VNC_PORT="5901" \
	NOVNC_PORT="6080" \
	DISPLAY=":1" \
	LANG="en_US.UTF-8" \
	LANGUAGE="en_US.UTF-8"


# ### qt 6 for debian bullseye
# RUN echo 'deb http://deb.debian.org/debian bullseye-backports main' | tee /etc/apt/sources.list.d/bullseye-backports.list \
#     && apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends -t bullseye-backports qt6-base-dev



### install mamba and environment packages from environment.yml
RUN conda install -n base -c conda-forge mamba
COPY environment.yml* .devcontainer/noop.txt /tmp/conda-tmp/
RUN if [ -f "/tmp/conda-tmp/environment.yml" ]; then umask 0002 && /opt/conda/bin/mamba env update -n base -f /tmp/conda-tmp/environment.yml; fi \
    && rm -rf /tmp/conda-tmp


### finalize
ENTRYPOINT ["/usr/local/share/desktop-init.sh"]
CMD ["sleep", "infinity"]
