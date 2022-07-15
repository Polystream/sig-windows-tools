# Note this image doesn't really mater for hostprocess
# the files in the image are copied to $env:CONTAINER_SANDBOX_MOUNT_POINT on the host
# but the file system is the Host NOT the container
FROM mcr.microsoft.com/windows/servercore:20H2

#FROM --platform=linux/amd64 curlimages/curl as bins
ARG containernetworkingCniVersion="1.1.1"
ARG cniVersion="0.3.0"
ARG flannelVersion="v0.18.1"

# Todo simplify this
# We need both sets of binaries 
# sdnoverlay from windows-container-networking is requred for containerd
# flannel.exe and hostlocal.exe from containernetworking 
# flannel.exe recently moved to https://github.com/flannel-io/cni-plugin but has no releases
WORKDIR /cni
RUN curl -Lo cni.tgz https://github.com/containernetworking/plugins/releases/download/v1.0.0/cni-plugins-windows-amd64-v1.0.0.tgz
RUN tar -xf cni.tgz
RUN del cni.tgz

RUN curl -Lo cni.zip https://github.com/microsoft/windows-container-networking/releases/download/v0.3.0/windows-container-networking-cni-amd64-v0.3.0.zip
RUN tar -xf /cni/cni.zip

WORKDIR /flannel 
RUN curl -Lo flannel-windows.tgz https://github.com/flannel-io/cni-plugin/releases/download/v1.1.0/cni-plugin-flannel-windows-amd64-v1.1.0.tgz
RUN tar -xf flannel-windows.tgz

#FROM $BASE

#ENV PATH="C:\Windows\system32;C:\Windows;C:\WINDOWS\System32\WindowsPowerShell\v1.0\;"

ADD https://raw.githubusercontent.com/microsoft/SDN/master/Kubernetes/windows/hns.psm1 /flannel/hns.psm1
COPY start.ps1 /flannel/start.ps1
# COPY --from=bins /cni /cni
# COPY --from=bins /flannel/flanneld.exe /flannel/flanneld.exe

ENTRYPOINT ["PowerShell", "/c", "$env:CONTAINER_SANDBOX_MOUNT_POINT/flannel/start.ps1"]