# Community Collections (CC)
A Research Computing Framework for Software Sharing

## Motivation and Citation

A recent PDF should be available [here](https://ssl.linklings.net/conferences/pearc/pearc19_program/views/includes/files/pap120s3-file1.pdf).

Citing our work increases our visibility to our scientific research community!  
Please include the following citation in related efforts:

[K. Manalo, L. Baber, R. Bradley, Z. You, N. Zhang. "Community Collections: A Framework for Openly Sharing Software Stacks Across Research Computing Centers Using Singularity and Lmod". PEARC19, July 28 - August 1, 2019, Chicago, IL, USA. DOI: 10.1145/3332186.3332199](https://doi.org/10.1145/3332186.3332199)

## Requirements

* Linux
* A root-installed Singularity or else we will install one for you, and give you the opportunity to enable it with root
* On RHEL7 systems, “Development Tools” including the gcc compiler
* `bzip2` for obvious reasons
* `git` to get the code
* `wget` to get necessary components
* The cryptsetup package for installing recent versions of Singularity
* A kernel with user namespaces if you lack root and wish to use Singularity sandbox containers instead of image files

CC has only been tested on Bash shells currently.

## Quick Start

See our longer docs here: https://community-collections.github.io/ Otherwise, below is a 'Quick Start'.

### Testing as a user with admin privileges

Use the following commands to test the code.

```
# obtain community collections
git clone http://github.com/community-collections/community-collections
cd community-collections

# optional: clean ups if you intend to start over
./cc clean # only if you are developing and want to delete everything
# clear your own cache if developing
rm -rf ~/.singularity ~/.cc_images 

# start here for the first time
./cc refresh
# we generate a cc.yaml so please have a look!
#   cc.yaml: remove error notes to build Lmod and Singularity locally if needed
vi cc.yaml 
# run cc refresh again after updating file
./cc refresh

# source initialization, user needs to affirm how cc is loaded into their environment
#   'cc profile' generates profile_cc.sh and adds it to ~/.bashrc (only contains references to Lmod)
./cc profile  
# if you wish to skip bashrc changes and only make the profile_cc.sh, use ./cc profile --no-bashrc
# source the file to enable cc
source profile_cc.sh # or get a new login shell if you said "y" to adding to your bashrc

# cc is live after you source a related file or agree to login shell mods
ml av # cc/conda supplies miniconda; cc/env supplies the conda env; and singularity is available as module

# if you are not root you may need to check if singularity is capable
./cc capable
sudo ./cc enable # sudo is required for sif files (no switch yet to enable sandboxes if you have userns)

# some examples of loading modules, community collections will 'pull' the image with the 
# combined power of Singularity and Lmod !
ml julia      # triggers the example singularity pull from docker
ml tensorflow # pulls a specific version with a suffix (see the default cc.yaml)
ml R          # gets a copy of R from r-base
```

Version checking and versionless modules are still under development.

### Testing in a docker container

This section describes a Docker environment which would be sufficient for
supporting CC.  The code is currently tested in a Docker container with a very
minimal set of requirements:

```
FROM centos:centos7
RUN yum update -y
RUN yum groupinstall -y 'Development Tools'
RUN yum install -y wget
RUN yum install -y which
RUN yum install -y vim
RUN yum install -y git
RUN yum install -y make
RUN yum install -y bzip2
RUN yum install -y cryptsetup
```

On a fresh VM with CentOS7:

```
# as root
yum update -y
yum groupinstall -y 'Development Tools'
yum install -y wget which vim git make bzip2 cryptsetup
```

Without `libtcl` or `squashfs-tools`, the code uses the `conda` environment to
supply these. 

Note that very recent testing shows that cryptsetup is now required for later
versions of Singularity 3.

## Documentation

The documentation source is located in `docs/source` and can be compiled
locally with `./cc docs`. Note that the administrators can update the
[documentation site](https://community-collections.github.io) with the `./cc
docs --push` command.
