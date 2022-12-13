# test-atlas
Notes on setting up a mini atlas for testing the pipeline extension, on a remote server using la-toolkit and microstack.

We loosely follow [Setup a LA demo using microstack](https://github.com/AtlasOfLivingAustralia/documentation/wiki/Setup-a-LA-demo-using-microstack).

## Setting up VM(s) on a remote Ubuntu (Linux) server, using [Microstack](https://microstack.run/docs/single-node)

1. Install microstack following the instructions in: https://microstack.run/docs/single-node
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
2. 


## Install the la-toolkit in your computer: https://github.com/living-atlases/la-toolkit#prerequisites

### On Mac

### On Linux
