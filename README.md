# test-atlas
Notes on setting up a mini atlas for testing the pipeline extension, on a remote server using la-toolkit and microstack.

We loosely follow [Setup a LA demo using microstack](https://github.com/AtlasOfLivingAustralia/documentation/wiki/Setup-a-LA-demo-using-microstack).

## Setting up VM(s) on a remote Ubuntu (Linux) server, using [Microstack](https://microstack.run/docs/single-node)

1. Install microstack in the *remote* machine following the instructions in: https://microstack.run/docs/single-node
```
# Install
sudo snap install microstack --beta
# Initialize
sudo microstack init --auto --control
# Verification
microstack.openstack image list
# Create an instance
microstack launch cirros -n test
# Access the instance (note that the IP number might differ)
ssh -i /home/ubuntu/snap/microstack/common/.ssh/id_microstack cirros@10.20.20.199
```

2. Connect to admin tool (Cloud dashboard) from your *local* machine

* Port forward the server's admin interface to your local machine

```
ssh  -N -L 8001:10.20.20.1:443 *remote*
```

You can now open the Cloud dashboard from a browser in your *local* machine at: https://localhost:8001.

Username: admin

You get the password with the below command in the *remote* machine.

```
sudo snap get microstack config.credentials.keystone-password
```

3. Create a Ubuntu 20.04 image to use for VMs

Download an image from https://cloud-images.ubuntu.com/.
The Ubuntu 20.04 image we used was: https://cloud-images.ubuntu.com/focal/current/focal-server-cloudimg-amd64.img

Add the image by clicking "Images" in the left column of the Cloud dashboard, and then "+ Create image".
Call the image "ubuntu20.04" and use "QCOW2 - QEMU Emulator" as "Format".

4. Create an instance of that image

In the *remote* machine run:

```
microstack launch ubuntu20.04 -n la-test-1 --flavor 4 
# Test access (*note* that your IP number might be different)
ssh -i /home/youruser/snap/microstack/common/.ssh/id_microstack ubuntu@10.20.20.200
```

5. Set up access to the VM from your local machine

Add your *public* key to the VMs `.ssh/authorized_keys`.
(For this, you need to logon to the VM from the *remote* machine, then edit the file.)
(You can also add your key to "Key pairs" in the Cloud dashboard. If this means that the keys will be added to new and/or existing VMs was not tested by us.)

Add the below -- change `sequoia` to the name of your *remote* machine, and type the correct IP number to the VM -- to your `.ssh/config` in your *local* machine:
```
Host la-test-sequoia
    HostName 10.20.20.200
    User ubuntu
    ProxyJump sequoia
```

Logon with:
```
ssh la-test-sequoia
```

## Install the la-toolkit in your computer: https://github.com/living-atlases/la-toolkit#prerequisites

Make sure you have Docker and Docker compose installed.
You need to be able to run Docker as non-root user.

### Install Docker and Docker Compose on Mac

### Install Docker and Docker Compose on Linux

See instructions at: https://docs.docker.com/engine/install/ubuntu/.
I used instructions under: https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository

1. Create a directory structure for `la-toolkit`

The default is to create directories starting at `/data`.
This might not work at all (OS X) or be inconvenient, and you might prefer to create directories in your home, e.g. `~/la-toolkit`.

2. Download `la-toolkit` files and start Docker

Download [docker-compose.yml](https://github.com/living-atlases/la-toolkit/blob/master/docker-compose.yml) and [mongo-init.sh](https://github.com/living-atlases/la-toolkit/blob/master/mongo-init.sh)
from the `la-toolkit` repo.
If you didn't choose the default paths for your directory structure, edit `docker-compose.yml` to add the correct paths.

Start the service:
```
docker-compose up -d
```

The toolkit is now listening on http://localhost:2010/.
User your browser to interact with it.
