# You can pick any Debian/Ubuntu-based image. 😊
FROM mcr.microsoft.com/devcontainers/base:ubuntu

# Options for setup script
ARG INSTALL_ZSH="true"
ARG UPGRADE_PACKAGES="false"
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID
# Install needed packages and setup non-root user. Use a separate RUN statement to add your own dependencies.
COPY library-scripts/*.sh /tmp/library-scripts/
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && /bin/bash /tmp/library-scripts/common-debian.sh "${INSTALL_ZSH}" "${USERNAME}" "${USER_UID}" "${USER_GID}" "${UPGRADE_PACKAGES}" "true" "true" \
    && apt-get install -y pkg-config zip g++ zlib1g-dev unzip

# Install Bazel
ARG BAZELISK_VERSION=v1.18.0
ARG BAZELISK_DOWNLOAD_SHA=dev-mode
RUN curl -fSsL -o /usr/local/bin/bazelisk https://github.com/bazelbuild/bazelisk/releases/download/${BAZELISK_VERSION}/bazelisk-linux-amd64 \
    && ([ "${BAZELISK_DOWNLOAD_SHA}" = "dev-mode" ] || echo "${BAZELISK_DOWNLOAD_SHA} */usr/local/bin/bazelisk" | sha256sum --check - ) \
    && chmod 0755 /usr/local/bin/bazelisk

# Installing ibazel
RUN cd /tmp && git clone https://github.com/bazelbuild/bazel-watcher.git \
    && cd bazel-watcher \
    && bazelisk build //cmd/ibazel \
    && mv $PWD/bazel-bin/cmd/ibazel/ibazel_/ibazel /usr/local/bin/ \
    && cd ~ && rm -rf /tmp/bazel-watcher

# Installing Chrome for testing
RUN apt-get install -y adwaita-icon-theme alsa-topology-conf alsa-ucm-conf at-spi2-core dconf-gsettings-backend dconf-service fontconfig fontconfig-config fonts-liberation \
        gsettings-desktop-schemas gtk-update-icon-cache hicolor-icon-theme humanity-icon-theme libasound2 libasound2-data libatk-bridge2.0-0 libatk1.0-0 libatk1.0-data \
        libatspi2.0-0 libauthen-sasl-perl libavahi-client3 libavahi-common-data libavahi-common3 libcairo-gobject2 libcairo2 libclone-perl libcolord2 libcups2 libdata-dump-perl \
        libdatrie1 libdconf1 libdeflate0 libdrm-amdgpu1 libdrm-common libdrm-intel1 libdrm-nouveau2 libdrm-radeon1 libdrm2 libencode-locale-perl libepoxy0 libfile-basedir-perl \
        libfile-desktopentry-perl libfile-listing-perl libfile-mimeinfo-perl libfont-afm-perl libfontconfig1 libfontenc1 libfreetype6 libfribidi0 libgbm1 libgdk-pixbuf-2.0-0 \
        libgdk-pixbuf2.0-bin libgdk-pixbuf2.0-common libgl1 libgl1-amber-dri libgl1-mesa-dri libglapi-mesa libglvnd0 libglx-mesa0 libglx0 libgraphite2-3 libgtk-3-0 libgtk-3-bin \
        libgtk-3-common libgtkd-3-0 libharfbuzz0b libhtml-form-perl libhtml-format-perl libhtml-parser-perl libhtml-tagset-perl libhtml-tree-perl libhttp-cookies-perl \
        libhttp-daemon-perl libhttp-date-perl libhttp-message-perl libhttp-negotiate-perl libice6 libio-html-perl libio-socket-ssl-perl libio-stringy-perl \
        libipc-system-simple-perl libjbig0 libjpeg-turbo8 libjpeg8 liblcms2-2 libllvm11 libllvm15 liblwp-mediatypes-perl liblwp-protocol-https-perl libmailtools-perl \
        libnet-dbus-perl libnet-http-perl libnet-smtp-ssl-perl libnet-ssleay-perl libnspr4 libnss3 libpango-1.0-0 libpangocairo-1.0-0 libpangoft2-1.0-0 libpciaccess0 \
        libphobos2-ldc-shared98 libpixman-1-0 libpng16-16 librsvg2-2 librsvg2-common libsensors-config libsensors5 libsm6 libtext-iconv-perl libthai-data libthai0 \
        libtie-ixhash-perl libtiff5 libtimedate-perl libtry-tiny-perl libu2f-udev liburi-perl libvte-2.91-0 libvte-2.91-common libvted-3-0 libvulkan1 libwayland-client0 \
        libwayland-cursor0 libwayland-egl1 libwayland-server0 libwebp7 libwww-perl libwww-robotrules-perl libx11-6 libx11-data libx11-protocol-perl libx11-xcb1 libxau6 libxaw7 \
        libxcb-dri2-0 libxcb-dri3-0 libxcb-glx0 libxcb-present0 libxcb-randr0 libxcb-render0 libxcb-shape0 libxcb-shm0 libxcb-sync1 libxcb-xfixes0 libxcb1 libxcomposite1 \
        libxcursor1 libxdamage1 libxdmcp6 libxext6 libxfixes3 libxft2 libxi6 libxinerama1 libxkbcommon0 libxkbfile1 libxml-parser-perl libxml-twig-perl libxml-xpathengine-perl \
        libxmu6 libxmuu1 libxpm4 libxrandr2 libxrender1 libxshmfence1 libxt6 libxtst6 libxv1 libxxf86dga1 libxxf86vm1 mesa-vulkan-drivers perl-openssl-defaults session-migration \
        tilix tilix-common ubuntu-mono ucf x11-common x11-utils x11-xserver-utils xdg-utils xkb-data \
    && curl -fSsL -o google-chrome-stable_current_amd64.deb https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb \
    && apt-get install -y ./google-chrome-stable_current_amd64.deb \
    && rm google-chrome-stable_current_amd64.deb

# [Optional] Uncomment this section to install additional OS packages.
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-list-here>

# Cleanup
RUN apt-get autoremove -y && apt-get clean -y && rm -rf /var/lib/apt/lists/* /tmp/library-scripts