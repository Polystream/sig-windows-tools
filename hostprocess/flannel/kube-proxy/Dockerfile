# Note this image doesn't really mater for hostprocess
# the files in the image are copied to $env:CONTAINER_SANDBOX_MOUNT_POINT on the host
# but the file system is the Host NOT the container
ARG BASE="mcr.microsoft.com/windows/servercore:20H2"

# FROM --platform=linux/amd64 curlimages/curl as bins
FROM $BASE

WORKDIR /kube-proxy
RUN curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.24.0/bin/windows/amd64/kube-proxy.exe
RUN curl -LO https://raw.githubusercontent.com/microsoft/SDN/master/Kubernetes/windows/hns.psm1



ENV PATH="C:\Windows\system32;C:\Windows;C:\WINDOWS\System32\WindowsPowerShell\v1.0\;"

ADD start.ps1 /kube-proxy/start.ps1
# COPY --from=bins /kube-proxy /kube-proxy

ENTRYPOINT ["PowerShell", "/c", "$env:CONTAINER_SANDBOX_MOUNT_POINT/kube-proxy/start.ps1"]