FROM --platform=$BUILDPLATFORM ubuntu:20.04

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update && \
    apt-get install -y tzdata bash curl file git unzip xz-utils zip libglu1-mesa build-essential cmake ninja-build lld llvm clang pkg-config libgtk-3-dev libglib2.0-dev libegl1-mesa-dev libxkbcommon-dev libgles2-mesa-dev flatpak flatpak-builder rsync && \
    rm -rf /var/lib/apt/lists/*

RUN groupadd -r -g 1441 flutter && useradd --no-log-init -r -u 1441 -g flutter -m flutter

USER flutter:flutter

WORKDIR /home/flutter

ARG flutterVersion=stable

ADD https://api.github.com/repos/flutter/flutter/compare/${flutterVersion}...${flutterVersion} /dev/null

RUN git clone https://github.com/flutter/flutter.git -b ${flutterVersion} flutter-sdk

RUN flutter-sdk/bin/flutter precache

RUN flutter-sdk/bin/flutter config --no-analytics

ENV PATH="$PATH:/home/flutter/flutter-sdk/bin"
ENV PATH="$PATH:/home/flutter/flutter-sdk/bin/cache/dart-sdk/bin"

RUN flutter doctor
RUN flutter config --enable-linux-desktop
RUN flatpak --user remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
RUN flatpak install flathub org.gnome.Platform//40 org.gnome.Sdk//40 -y
