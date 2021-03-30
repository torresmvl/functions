FROM ghcr.io/openfaas/classic-watchdog:0.1.5 as watchdog

FROM alpine:3.12 as drill

RUN adduser inkscape -S -D drill
RUN apk --update add --no-cache drill

FROM scratch as release

# Populate example here - i.e. "cat", "sha512sum" or "node index.js"
ENV fprocess="xargs drill"
# Set to true to see request in function logs
ENV write_debug="false"

COPY --from=drill / /
COPY --from=watchdog /fwatchdog /usr/bin/fwatchdog

USER drill
WORKDIR /home/drill
HEALTHCHECK --interval=3s CMD [ -e /tmp/.lock ] || exit 1
CMD ["fwatchdog"]
