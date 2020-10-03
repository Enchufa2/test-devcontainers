# [Choice] Ubuntu version: bionic, focal
ARG VARIANT=focal
FROM mcr.microsoft.com/vscode/devcontainers/base:${VARIANT}

# Options for setup script
ARG INSTALL_ZSH="true"
ARG UPGRADE_PACKAGES="true"
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Install needed packages and setup non-root user. Use a separate RUN statement to add your own dependencies.
COPY library-scripts/*.sh /tmp/library-scripts/
RUN bash /tmp/library-scripts/common-debian.sh "${INSTALL_ZSH}" "${USERNAME}" "${USER_UID}" "${USER_GID}" "${UPGRADE_PACKAGES}" \
    && rm -rf /tmp/library-scripts

RUN echo "es_ES.UTF-8 UTF-8" >> /etc/locale.gen \
	&& locale-gen

RUN echo 'APT::Install-Recommends "false";' > /etc/apt/apt.conf.d/90local-no-recommends \
    && apt-get -y install software-properties-common \
    && add-apt-repository --enable-source --yes "ppa:marutter/rrutter4.0" \
    && add-apt-repository --enable-source --yes "ppa:c2d4u.team/c2d4u4.0+" \
    && apt-get -y autoremove software-properties-common

RUN apt-get -y install \
        r-base \
        r-base-dev \
        r-recommended \
        r-cran-bspm \
        python3-apt \
    && echo "bspm::enable()" >> /etc/R/Rprofile.site \
    && echo "options(bspm.sudo=TRUE)" >> /etc/R/Rprofile.site \
    && Rscript -e 'install.packages(tools::package_dependencies("languageserver", recursive=TRUE)[[1]])' \
    && Rscript -e 'install.packages("languageserver")'

USER vscode
