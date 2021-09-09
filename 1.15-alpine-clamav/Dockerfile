FROM golang:1.15-alpine
LABEL maintainer="Frank Wilkerson"

RUN apk add --no-cache gcc musl-dev git bash
RUN wget -O- -nv https://raw.githubusercontent.com/golangci/golangci-lint/master/install.sh | sh -s v1.38.0
RUN mv /go/bin/golangci-lint /usr/local/bin/
RUN wget -O /usr/local/bin/swagger "https://github.com/go-swagger/go-swagger/releases/download/v0.26.1/swagger_linux_amd64"
RUN chmod +x /usr/local/bin/swagger
RUN wget -O /usr/local/bin/go-acc "https://github.com/fwilkerson/go-acc/releases/download/v0.2.6/go-acc_linux_amd64"
RUN chmod +x /usr/local/bin/go-acc

ENV FILE_SIZE = "500M";

# Add Clamav to the image

RUN addgroup -g 2001 clamav && adduser -u 2001 -G clamav -h /var/lib/clamav -D -s /sbin/nologin clamav

RUN apk --no-cache add clamav \
    && mkdir /run/clamav \
    && chown clamav:clamav /run/clamav

RUN freshclam --quiet

RUN echo "TCPSocket 3310" >> /etc/clamav/clamd.conf && \
    echo "TCPAddr 127.0.0.1" >> /etc/clamav/clamd.conf && \
    sed -i 's/^MaxScanSize.*$/MaxScanSize $FILE_SIZE/' /etc/clamav/clamd.conf && \
    sed -i 's/^MaxFileSize.*$/MaxFileSize $FILE_SIZE/' /etc/clamav/clamd.conf && \
    sed -i 's/^StreamMaxLength.*$/StreamMaxLength $FILE_SIZE/' /etc/clamav/clamd.conf && \
    mkdir /unscanned_files

RUN freshclam --quiet

