################################################################################
# Add-on: HAOS Kiosk Display (haoskiosk)
# File: Dockerfile
# Version: 1.3.1
# Copyright Jeff Kosowsky
# Date: April 2026
################################################################################

ARG BUILD_FROM=homeassistant/amd64-base:latest
FROM $BUILD_FROM

ARG BUILD_VERSION
ENV ADDON_VERSION=${BUILD_VERSION}

#===============================================================================
##### Install X, Browser and all necessary dependencies
RUN apk update && apk upgrade && apk add --no-cache \
     xorg-server \
     xf86-video-modesetting \
     mesa-dri-gallium \
     mesa-egl \
     mesa-gles \
     libdrm \
     xdotool \
     xinput \
     xrandr \
     xset \
     libxkbcommon \
     setxkbmap \
     xf86-input-libinput \
     libinput \
     udev \
     libinput-udev \
     libevdev \
     evtest \
     openbox \
     onboard \
     unclutter-xfixes \
     ttf-dejavu \
     bash \
     util-linux \
     patch \
     scrot \
     icu-data-full \
     py3-pip \
     py3-xlib \
     pulseaudio-utils \
     x11vnc \
     chromium \
     intel-media-driver \
     libva-intel-driver \
     mesa-va-gallium \
     dbus \
     dbus-x11 \
     nss \
     nspr \
     freetype \
     ttf-freefont \
     font-noto \
     libxml2 \
     libxslt

     
   && rm -rf /var/cache/apk/*

#===============================================================================

##### Set the display variable
ENV DISPLAY=:0

##### Copy over 'xorg.conf.default' and lua 'userconf.lua' file
COPY xorg.conf.default /etc/X11/
COPY userconf.lua /root/.config/luakit/
COPY translations/*.yaml /translations/

COPY run.sh /
RUN chmod a+x /run.sh

COPY mouse_touch_inputs.py gesture_commands.json /
COPY rest_server.py /
RUN pip install --no-cache-dir  --break-system-packages aiohttp #Required for rest_server.py

#### Patches
# Need to patch 'unique_instance.lua' so that new instance urls overwrite active url rather than add new tab
# COPY unique_instance.patch /usr/share/luakit/lib
# RUN patch -p2 /usr/share/luakit/lib/unique_instance.lua < /usr/share/luakit/lib/unique_instance.patch

CMD ["/run.sh"]
