FROM ghcr.io/openfaas/classic-watchdog:0.1.5 as watchdog

FROM alpine:3.12 as inkscape

RUN adduser inkscape -S -D inkscape
RUN apk --update add --no-cache inkscape

FROM scratch as release

# Populate example here - i.e. "cat", "sha512sum" or "node index.js"
ENV fprocess="xargs inkscape"
# Set to true to see request in function logs
ENV write_debug="false"

COPY --from=inkscape / /
COPY --from=watchdog /fwatchdog /usr/bin/fwatchdog

USER inkscape
WORKDIR /home/inkscape
HEALTHCHECK --interval=3s CMD [ -e /tmp/.lock ] || exit 1
CMD ["fwatchdog"]
