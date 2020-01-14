# Visual Studio Code

This directory contains configuration for Visual Studio Code remote development and debugging.

You can launch a docker container and then edit the code of this repo in that container and launch it in a debugger.

In order to use this on a mac with docker for mac, do the following:


You need to make sure, that the process in the container has access to a libvirtd running on some hypervisor. You can do that
by creating an ssh tunnel to it. This will then expose the port on you mac and thus make it accessible from the container.

    ssh <hypervisor> -L 0.0.0.0:16509:localhost:16509

test with the following command on the mac:

    virsh -c qemu+tcp://localhost:16509/system list


Now start Docker for mac and launch Visual Studio Code remote debugging with the Dockerfile in this directory.

Assuming, that your docker for mac instance uses the network: 192.168.65.0/24, then the IP of your mac is most likely 192.168.65.2 on that network.
Therefore, it should be possible to run

    virsh -c qemu+tcp://192.168.65.2:16509/system list

from a shell in the docker container.

You can run libvirt-exporter manually in docker with:

    ./libvirt-exporter --libvirt.uri="qemu+tcp://192.168.65.2:16509/system"

in order to test the exporter run in a shell in the container:

    wget -qO - http://localhost:9177/metrics

or get the ip of the container and then run from your mac: (ip may be different)

    curl http://192.168.65.3:9177/metrics

