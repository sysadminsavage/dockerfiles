# Usage:
# X11: xhost +"local:docker@"
# docker run --log-driver none --rm \
#         -v /dev/shm:/dev/shm \
#         -v /tmp/.X11-unix:/tmp/.X11-unix:ro \
#         -e DISPLAY=$DISPLAY \
#         wfnintr/torbrowser
#
# Wayland: x11docker wfnintr/torbrowser
FROM debian:trixie-slim
LABEL maintainer="sysadminsavage@protonmail.com"

COPY torbrowser.tar.xz /opt/tor/

RUN apt-get update && apt-get install -y curl gnupg file xz-utils \
	libgtk-3.0 libdbus-glib-1-2 libx11-xcb-dev libxt6 libasound2

ENV TOR_VERSION="13.5a8" \
	FINGERPRINT="EF6E286DDA85EA2A4BA7DE684E2C6E8793298290"

# Setup tor install path
WORKDIR /opt/tor

# Download torbrowser. Change ${TOR_VERSION} above to specify which version to download
RUN curl -s https://dist.torproject.org/torbrowser/${TOR_VERSION}/tor-browser-linux-x86_64-${TOR_VERSION}.tar.xz -o torbrowser.tar.xz

# Download checksum
RUN curl -s https://dist.torproject.org/torbrowser/${TOR_VERSION}/tor-browser-linux-x86_64-${TOR_VERSION}.tar.xz.asc -o torbrowser.tar.xz.asc

# Verify checksum
RUN gpg --auto-key-locate nodefault,wkd --locate-keys torbrowser@torproject.org && \
        gpg --output ./tor.keyring --export ${FINGERPRINT} && \
        gpgv --keyring ./tor.keyring /opt/tor/torbrowser.tar.xz.asc /opt/tor/torbrowser.tar.xz 


# Extract
RUN tar --strip 1 -xJf torbrowser.tar.xz tor-browser/Browser

ENV LANG C.UTF-8

# Switch User
RUN useradd anon 
RUN chown -R anon:anon /opt/tor
USER anon

# Run
ENTRYPOINT ["/bin/bash"]
CMD ["/opt/tor/Browser/start-tor-browser", "--log", "/dev/stdout" ]
