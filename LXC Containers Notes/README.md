## ABOUT LXC Containers

LXC Containers use LXC Toolset and LXD API to communicate with the Host OS and are considered to be more efficient as they do not need any container runtime and are equivalent to installing Virtual Machines without a Hypervisor on top of a BM Server

LXD and LXC are already installed on any Linux Based Distros, if not, they can be installed with the help of native package Managers (like yum or apt etc)

Important Note:

if lxc based commands are to be run without the use of sudo, then it is beneficial to add your user to the lxd group in your system
You may use below command for that:

```sudo gpasswd -a <Your_User> lxd```

Run the ```groups``` command post this, to check if your user is a part of the lxd group or not, if not, try relogging into the server


## SOME COMMANDS 

##### Initialise the lxc, would ask to enable cluster (to form lxc cluster), select relevant option 
    lxc init 

##### To Check version of server and client
    lxc version

##### Help Section
    lxc help

##### To List Available Storages
    lxc storage list

##### To get a list of available remote repos
    lxc remote list

##### To get a list of avaiable images in system
    lxc image list

##### To search for images in remote repos
    lxc image list image:<search_pattern_for_image>

##### To list the running conatiners
    lxc list

##### To Launch a container
    lxc launch images/<image_name>:<version> <name_you_want_to_reference_container>

##### To copy an image from one name to another
    lxc copy <first_name> <new_name>

##### To move container from one host to another (This can only function in an LXD Cluster)
    lxc move host:conatiner new_host:conatiner

##### To Get a bash shell inside conatiner (running)
    lxc exec <container_name> bash/zsh/ksh

##### Get info on Container
    lxc info <container_name>

##### View Configuration of Container, modify/set threshold on mem and cpu usage
    lxc config <container_name>
    lxc config set <container_name> limits.memory <New_value>
    lxc config set <container_name> limits.cpu <New_value>

##### Each conatiner is mapped to a profile and below commands can be use for that pupose
    lxc profile show <profile_name/default>
    lxc profile list
    lxc profile create <profile_name>
    lxc profile edit <profile_name>
    lxc launch image_name:version container_name --profile <profile_to_be_mapped_to>

##### To push/pull a file inside/from container
    lxc file push <file_name> <container_name>/<path_to_file>
    lxc file pull <file_name> <container_name>/<path_to_file>

##### To secure snapshot of any container
    lxc snapshot <container_name> <snapshot_name>
    lxc restore <container_name> <snapshot_name>

##### To setup nested containers, you need to set the below parsmeters in parent container to true, if not present add them under limits section
    security.priviledged: true
    security.nesting: true


