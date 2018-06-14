###------------------------------------------------------------------------
### Originally based off:
### https://github.com/efcasado/dockerfile-ocaml/blob/master/Dockerfile
###
##== Preamble =============================================================
ARG        ALPINE_VERSION=3.6
FROM       alpine:${ALPINE_VERSION}
MAINTAINER Patrick Stapfer <p.stapfer@gmail.com>
ARG        OCAML_MAJOR=4.06
ARG        OCAML_VERSION=4.06.0
ARG        OPAM_VERSION=1.2.2

##== Layers ===============================================================

RUN apk --no-cache add \
      build-base \
      wget \
      openssl \
      ca-certificates \
      bash \
      m4 \
      git \
      rsync \
      mercurial \
      patch \
      perl \
      aspcud \
      sudo && \
      adduser -D -u 1000 opam && \
      echo "opam ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers.d/default

# OCaml and OPAM
RUN wget http://caml.inria.fr/pub/distrib/ocaml-${OCAML_MAJOR}/ocaml-${OCAML_VERSION}.tar.gz && \
    tar xzf ocaml-${OCAML_VERSION}.tar.gz && \
    cd ocaml-${OCAML_VERSION} && \
    ./configure && make world.opt && umask 022 && make install && make clean && \
    cd .. && \
    rm -f ocaml-${OCAML_VERSION}.tar.gz && \
    rm -rf ocaml-${OCAML_VERSION} && \
    wget https://github.com/ocaml/opam/releases/download/${OPAM_VERSION}/opam-full-${OPAM_VERSION}.tar.gz && \
    tar zxf opam-full-${OPAM_VERSION}.tar.gz && \
    cd opam-full-${OPAM_VERSION} && \
    ./configure && OCAMLPARAM="safe-string=0,_" make lib-ext all install && \
    cd .. && \
    rm -f opam-full-${OPAM_VERSION}.tar.gz && \
    rm -rf opam-full-${OPAM_VERSION}

ENV HOME /home/opam
WORKDIR /home/opam
USER opam

RUN yes | opam init && \
    eval `opam config env` && \
    yes | opam install utop

ENTRYPOINT [ "bash", "-l", "-c" ]
