# Cumulus Compute Suggestions
Docs on accessing development environments on the ARM Cumulus Cluster

## Running Jupyter Notebooks on Cumulus
Familiar with Python? Do you often use Jupyter Notebooks within your workflow?
Be sure to checkout the following documentation on running a notebook remotely.


### Install Jupyter and Create Directory

1) Make sure that Jupyterlab is installed in your conda environment by typing conda install jupyter notebook and following the installation instructions on screen.

3) Come up with a port number (ex. 4444).  Keep this port number in mind as we’ll use it throughout the process to pass the web browser interface for Jupyter from the 
compute node back to your local machine through the firewall using an ssh tunnel.
```
ssh armID@cumulus.ccs.ornl.gov
```

### Setup your Jupyter Notebook on Cumulus

4) From a terminal window on MacOS, or a Windows PowerShell session on Windows 10, ssh to Cumulus

5) Start a bash session (to be able to run python):
<pre><code> bash </code></pre>

6) Start an interactive session on one of Cumulus’s compute nodes using the qlogin command
```
salloc -A PROJECT -p batch_all -N 1 -t 12:00:00
```

This will request a compute session for 12 hours on a single node.  If the resources are available, you will be logged into a session on one of Cumulus’s compute nodes, e.g., cirrus27.

*Note the machine name (i.e., cirrus27) that you are logged into, you’ll need it for a later step.*

7) Start Jupyter notebook using the following command:
```
jupyter lab --port=XXXX --no-browser --ip=127.0.0.1
```

Replace XXXX with the port number you came up with above.  If there is an error regarding the port (i.e., it says that it is already used), then try a different one.  If successful, the command will display a link that you will need to connect to the Jupyter notebook in the next step

*You will need the url it returns at the bottom of the terminal*

### Connect to your notebook using your local machine

8) Using terminal or PowerShell, open a second ssh session to Cumulus, except now we will open an ssh tunnel to the compute node using the port that we used in the previous step.
```
ssh -L XXXX:127.0.0.1:XXXX armID@cumulus.ccs.ornl.gov ssh -L XXXX:127.0.0.1:XXXX armID@remotemachine
```

You’ll have to replace
  * The port number from above in 4 places
  * Your netID in two places
  * The name of the remote machine (i.e., cirrus27; see step 5 under Install Jupyter and Create Directory for more details)

Note that you may have to enter your password if you don’t have the ssh key for Cumulus stored on your local machine.

This will set up an ssh tunnel from port xxxx (which is accessible from your web browser) to the Jupyter notebook server on Cumulus.  You will need to kill this ssh session (with Control-C) when you are finished, as you can’t connect again to this port without killing it explicitly.

9) Now, point a web browser to the URL that you obtained in Step 1.  You should see a Jupyter Lab environment that displays locally, but executes code on Cumulus, and has access to Cumulus file systems.  Note that it will not have access to your local file systems (e.g., on your Mac or PC).
