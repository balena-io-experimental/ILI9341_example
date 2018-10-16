FROM resin/rpi-raspbian AS build

RUN apt-get update &&  \
    apt-get install -y \
    build-essential \
    cmake \
    git \
    libraspberrypi-dev \
    raspberrypi-kernel-headers

WORKDIR /usr/src

RUN git clone https://github.com/juj/fbcp-ili9341.git && \
    mkdir /usr/src/fbcp-ili9341/build

WORKDIR /usr/src/fbcp-ili9341/build

RUN cmake \
    -DILI9341=ON \
    -DGPIO_TFT_DATA_CONTROL=22 \
    -DGPIO_TFT_RESET_PIN=27 \
    -DGPIO_TFT_BACKLIGHT=17 \
    -DSPI_BUS_CLOCK_DIVISOR=16 \
    -DBACKLIGHT_CONTROL=ON \
    -DSTATISTICS=0 \
    ..

RUN make -j

FROM resin/rpi-raspbian
COPY --from=build /usr/lib/arm-linux* /usr/lib/
COPY --from=build /opt/vc/lib/* /opt/vc/lib/
COPY --from=build /usr/src/fbcp-ili9341/build/fbcp-ili9341 /usr/bin/

CMD fbcp-ili9341
