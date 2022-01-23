# raspbian Base Image
FROM ubuntu:20.04 as intermediate

ARG version=0.9.25

# Update Repository
RUN apt-get update
# Upgrade Packets
RUN DEBIAN_FRONTEND=noninteractive apt-get upgrade -y
# install Dependencies
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends sudo locales libpython3.7 libssl1.1 libjansson4 libxml2 openssl libmariadb3 bsd-mailx msmtp msmtp-mta libcurl4 mosquitto mosquitto-clients libuuid1 python3 python3-mysql.connector wiringpi rsyslog libcap-dev
#Add German locales
RUN touch /usr/share/locale/locale.alias
RUN sed -i '/de_DE.UTF-8/s/^# //g' /etc/locale.gen && locale-gen

RUN DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends git build-essential pkg-config libssl-dev libjansson-dev libxml2-dev libcurl4-openssl-dev libssl-dev libmariadbclient-dev libmariadb-dev-compat uuid-dev cmake python3-dev wiringpi ca-certificates

WORKDIR /usr/src
RUN git clone https://libwebsockets.org/repo/libwebsockets libwebsockets
WORKDIR /usr/src/libwebsockets/build
RUN cmake ..
RUN make -s
RUN make -s install

WORKDIR /usr/src
RUN git clone -b $version https://github.com/horchi/linux-p4d/
WORKDIR /usr/src/linux-p4d
RUN make clean all
RUN mkdir -p /usr/src/docker-linux-p4d/root
RUN make install DESTDIR=/usr/src/docker-linux-p4d/root/ PREFIX=/usr





# real image for running p4d
FROM ubuntu:20.04

# Update Repository
RUN apt-get update
# Upgrade Packets
RUN DEBIAN_FRONTEND=noninteractive apt-get upgrade -y
# install Dependencies
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends sudo locales libpython3.7 libssl1.1 libjansson4 libxml2 openssl libmariadb3 bsd-mailx msmtp msmtp-mta libcurl4 mosquitto mosquitto-clients libuuid1 python3 python3-mysql.connector wiringpi rsyslog libcap-dev
#Add German locales
RUN touch /usr/share/locale/locale.alias
RUN sed -i '/de_DE.UTF-8/s/^# //g' /etc/locale.gen && locale-gen

COPY --from=intermediate /usr/src/docker-linux-p4d/root/ /

