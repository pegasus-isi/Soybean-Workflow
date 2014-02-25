Soybean-Workflow
================

This is an initial version of the Soybean workflow.

Note that the Git repository does not include the software required for
the jobs. For now, grab the software tarball from
http://www.isi.edu/~rynge/soybean/software.tar.gz . Untar it in the 
top level directory.

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

