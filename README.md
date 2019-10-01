# artdaq-docker

This docker image has been tested with podman under RHEL8. If you are using Scientific Linux 7.6 or above, you can install podman with:

```sh
sudo yum -y install yum-conf-extras
sudo yum -y install podman
```

Assuming you have a `/scratch` directory on your host OS and the user you logged in as have read/write access to it, you can start the container with (otherwise, change the volume mount to the full path to another directory you have read/write access to):

```sh
podman run  --privileged=true  -ti -e DISPLAY  --rm  -v /scratch:/scratch --net=host  dingpf/artdaq
```

In the container, you can do the following to run `artdaq-demo` (using `artdaq-demo v3_06_01` as an example):

```sh
cd /scratch
wget https://cdcvs.fnal.gov/redmine/projects/artdaq-demo/repository/revisions/develop/raw/tools/quick-mrb-start.sh
chmod +x quick-mrb-start.sh
# Pull UPS products and compile artdaq packages
./quick-mrb-start.sh --tag v3_06_01

# Setting up environment to run the demo
source setupARTDAQDEMO
setup artdaq_daqinterface v3_06_01
export DAQINTERFACE_USER_SOURCEFILE=$PWD/DAQInterface/user_sourcefile_example
source $ARTDAQ_DAQINTERFACE_DIR/source_me

# Now run the demo
ipcrm -a;./run_demo.sh --config demo --comps component{01..02} --no_om --runduration 20

```
