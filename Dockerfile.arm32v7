### in case of local build
#FROM golang:1.12.0 AS builder
#WORKDIR /builder/working/directory
#RUN curl -L https://github.com/balena-io/qemu/releases/download/v3.0.0%2Bresin/qemu-3.0.0+resin-arm.tar.gz | tar zxvf - -C . && mv qemu-3.0.0+resin-arm/qemu-arm-static .

FROM arm32v7/ubuntu:18.04
#COPY --from=builder /builder/working/directory/qemu-arm-static /usr/bin
ADD qemu-arm-static /usr/bin
RUN apt-get update -y && \
    apt-get install -y --no-install-recommends curl unzip cron man-db nano tzdata && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
RUN curl https://rclone.org/install.sh | bash -s beta
VOLUME ["/root/.config/rclone", "/etc/cron.d"]
ADD rclone.conf /root/.config/rclone/rclone.conf
ADD crontab /etc/cron.d/crontab
RUN touch /var/log/cron.log && /usr/bin/crontab /etc/cron.d/crontab && chmod 0644 /etc/cron.d/crontab
ENV TZ=Europe/Berlin
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
CMD ["cron", "-f"]
