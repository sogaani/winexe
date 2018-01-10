FROM alpine:3.6

ARG WINEXE_REPOSITORY="https://github.com/sogaani/winexe.git"
ARG WINEXE_BRANCH="master"

ARG SAMBA_REPOSITORY="git://git.samba.org/samba.git"
ARG SAMBA_HASH="fe164a08ddfee8efd802ed27a90f24e8afe2789a"
ARG SAMBA_DIR="/usr/local/samba_repo"

COPY patch/samba.patch /usr/local/samba.patch

RUN set -x &&\
	apk upgrade --update && \
        apk add \
		bash \
		git \
                'nodejs>=6.2.0' \
		nodejs-npm \
		tevent-dev \
		popt-dev \
		openssh \
		libc-dev \
                musl-dev \
                eudev-dev \
                libevent-dev \
                linux-headers \
                python-dev \
                gnutls-dev \
                acl-dev \
                openldap-dev \
                linux-pam-dev \
		python \
		rpcgen \
                git \
                make \
                gcc \
                g++ \
                autoconf \
                automake \
                wget \
                curl \
                sudo \
                tar \
		binutils-gold \
                gnupg \
		libbsd-dev \
                xz 

# samba
RUN set -x &&\
	# samba
	mkdir -p ${SAMBA_DIR} &&\
        git clone ${SAMBA_REPOSITORY} ${SAMBA_DIR} &&\
        cd ${SAMBA_DIR} &&\
	git reset --hard ${SAMBA_HASH} &&\
	patch -p1 < /usr/local/samba.patch

# build winexe
RUN set -x &&\
	# winexe
        mkdir -p /usr/local/winexe &&\
        git clone ${WINEXE_REPOSITORY} /usr/local/winexe &&\
	cd /usr/local/winexe/source \
	ln -s /lib/libcom_err.so.2 /lib/libcom_err.so &&\
	\./waf \
	\--samba-inc-dirs="/usr/local/samba/include ${SAMBA_DIR}/source4 ${SAMBA_DIR} ${SAMBA_DIR}/bin/default ${SAMBA_DIR}/bin/default/source4" \
	\--samba-dir=${SAMBA_DIR} \ 
	\configure build

WORKDIR /usr/local/winexe
COPY startup.sh /startup.sh

CMD [ "/startup.sh" ]
