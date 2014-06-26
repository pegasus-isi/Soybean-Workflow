Soybean-Workflow
================

Note that the Git repository does not include the software required for
the jobs. For now, grab the software tarball from
http://www.isi.edu/~rynge/soybean/software.tar.gz . Untar it in the 
woprkflow directory.

The workflow is controlled by a configuration file named
~/.soybean-workflow.conf . Create the file with this content:

```
# local refers to the submit host. Specify paths to a directory
# which can be used by the workflow as work space, and locations
# for local software installs.
[local]

work_dir = /local-scratch/%(username)s/soybean

irods_bin = /ccg/software/irods/3.2/bin

# tacc refers to configuration for the TACC Stampede 
# supercomputer. To use this machine, you need an allocation
# (start with TG-) and you also need to know your username
# and storage group name for the system. The easiest way to 
# obtain those is to log into the system, and run:
# cds; pwd
# This should return a path like: /scratch/00384/rynge. The
# storage group is the second level, and your username is 
# last level.
[tacc]

allocation = TG-ABC1234

username = rynge

storage_group = 00384

```

Basic files are pulled from the submit host with scp. This is to keep
the requirements on the submit host light, and make it easy to run the
submit host as a cloud instance in for example Atmosphere. A workflow
needs an ssh key:

```
mkdir -p ~/.ssh
ssh-keygen -t rsa -b 2048 -f ~/.ssh/workflow
     (just hit enter when asked for a passphrase)
cat ~/workflow.pub >>~/.ssh/authorized_keys 
```

To access data from the iPlant iRods repository, you need a file in your
home directory named ~/irods.iplant.env, with 0600 permission and
content like:

```
irodsHost data.iplantcollaborative.org
irodsPort 1247
irodsUserName YOUR_IRODS_USERNAME
irodsZone iplant
# Pegasus requirement
irodspassword 'YOUR_IRODS_PASSWORD'
```

To run on TACC, you need a X509 proxy. Create one with:

    myproxy-logon -s myproxy.teragrid.org -t 144:00 -l YOUR_XSEDE_USERNAME

To generate the workflow:

    ./workflow-generator --exec-env tacc-stampede

