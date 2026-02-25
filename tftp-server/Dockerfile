# Dockerfile - Custom TFTP server with tftpd-hpa
# Includes write support for receiving files from devices
FROM alpine:3.19

# Install the tftpd-hpa server
RUN apk add --no-cache tftp-hpa

# Create the TFTP root directory
RUN mkdir -p /var/tftpboot && chmod 777 /var/tftpboot

# Expose the standard TFTP port
EXPOSE 69/udp

# Run tftpd-hpa in the foreground
# --foreground keeps the process running for Docker
# --create allows clients to upload new files
# --secure restricts access to the specified directory
CMD ["in.tftpd", "--foreground", "--create", "--secure", "--verbose", "/var/tftpboot"]

# Source: https://oneuptime.com/blog/post/2026-02-08-how-to-run-tftp-server-in-docker/view