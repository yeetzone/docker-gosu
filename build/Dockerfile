FROM debian:10-slim as GOSU

# Set build arguments.
ARG GOSU_VERSION
ENV GOSU_VERSION ${GOSU_VERSION:-1.16}

# Install everything.
RUN set -ex \
	# Install dependencies.
	&& apt-get update \
	&& apt-get install --yes --no-install-recommends wget ca-certificates gnupg2 dirmngr \
	# Install gosu.
	&& wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$(dpkg --print-architecture)" \
	&& wget -O /usr/local/bin/gosu.asc "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$(dpkg --print-architecture).asc" \
	&& export GNUPGHOME="$(mktemp -d)" \
	&& gpg --keyserver hkps://keys.openpgp.org/ --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 \
	&& gpg --batch --verify /usr/local/bin/gosu.asc /usr/local/bin/gosu \
	&& rm -rf "$GNUPGHOME" /usr/local/bin/gosu.asc \
	&& chmod +x /usr/local/bin/gosu \
	&& gosu nobody true

FROM debian:10-slim
MAINTAINER YEET Zone <support@yeet.zone>

# Install gosu.
COPY --from=GOSU /usr/local/bin/gosu /usr/local/bin/gosu

# Set default command.
CMD ["gosu"]
