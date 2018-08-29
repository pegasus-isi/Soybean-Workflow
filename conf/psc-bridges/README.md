

## Editing sites.catalog.template

Please note that `sites.catalog.template` needs to be updated based on the user who will run the glideins on PSC Bridges. The section which needs to be updated is:

    <site  handle="execution" arch="x86_64" os="LINUX">
        <directory type="shared-scratch" path="/pylon5/bi560mp/rynge/workflow-runs">
            <file-server operation="all" url="file:///pylon5/bi560mp/rynge/workflow-runs"/>
        </directory>

Determine the shared directory assigned to you on Bridges by logging in and running `echo $SCRATCH`. Update the two paths in the section above such that they have your scratch directory, plus `/workflow-runs`. Do _not_ use environment variables here - only full expanded paths are allowed.


## Glideins on PSC Bridges

This setup is based on a PyGlidein setup (https://pegasus.isi.edu/documentation/pyglidein.php)

To get setup up, first log in to your PSC Bridges account, and then copy the configuration to your home directory:

    $ cd ~
    $ cp ~rynge/rnaseq ~/

Edit `~/rnaseq/config/rnaseq-bridges.config`. And the minimum, change `user` and the location of `tarball`, replacing `rynge` with your username. Also update the `#SBATCH --account=` line with a project you want the glidein to charge to.

Set up the Python virtual environment, and try submitting your first glidein (assuming you already have a workflow submitted on workflow.isi.edu - pyglidein will check for demand before submitting new glideins):

    $ module load python2
    $ cd ~/rnaseq/
    $ . venv/bin/activate
    $ pyglidein_client --config=$HOME/rnaseq/config/rnaseq-bridges.config --secrets=$HOME/rnaseq/config/secrets

The output should state that a glidein was submitted:

    2018-08-29 17:29:55,788 DEBUG {u'count': 1, u'cpus': 1, u'memory': 0, u'gpus': 0, u'disk': 0.001, u'os': None}
    2018-08-29 17:29:55,788 DEBUG {u'jsonrpc': u'2.0', u'result': [{u'count': 1, u'cpus': 1, u'memory': 0, u'gpus': 0, u'disk': 0.001, u'os': None}], u'id': 0}
    Submitted batch job 3865303
    2018-08-29 17:29:55,846 INFO launched 1 glideins on RM

After a few minutes, you should be able to see the glidein by running `condor_status` on `workflow.isi.edu`.

PSC Bridges no longer allows cron jobs, so you have to use the pyglidein_client to start glideins manually.

