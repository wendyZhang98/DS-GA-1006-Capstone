# DS-GA-1006-Capstone

### Goals of the Bootcamp
- Workflow: Github, Collaboration, Testing, Reproducibility
- Software tools: IDE, CLI, Environment Management(Docker/Singularity)
- System Knowledge: HPC, GPUs

### Data Science Software Stack
- CLI
- IDE(Vscode), Debugging, Testing, Notebook
- Python(Conda), Programming with GPUs

[Resource Covering a lot of the general software engineering material in more detail](https://missing.csail.mit.edu/)

[Practical Examples given by WendaZhou](https://github.com/wendazhou/cds-bootcamp)

### Shell
#### 1) Text-based interface for computer; Access through terminal
- Programmable/ Automatable
- Translate well across platforms(Laptop, Desktop, HPC Cluster, Cloud)
#### 2) Shell Basics
- Shell is a programming language
- Main programming aspects we make use of: variables, redirection, globbing
#### 3) Shell Variables
- All variables in Shell are "strings"
- Define a variable by assignment: MY_VAR="my value"
- Access variable value by expansions(string replacement): echo"$HOME/$MY_VAR"
- Double quotes(") enalble expansions, but single quotes(') don't!: echo'$MY_VAR' doesn't work
- Pass variables to prgram: export, otherwise value only set in your shell
#### 4) Shell Globbing
- *: match any number of characters: echo *.txt lists all text files 
- ?: match single character
- [1-9]: match any single digit
#### 5) Shell Redirection
- Programs can communicate with each other through standard pipes: standard output=1, standard error=2
- Redirect output to files: echo "Test" data 1>output.txt (redirect standard output to output.txt); python -m script.py 2>&1 (redirect standard error to standard output)
- Be careful about buffering: python -u *.py will run without buffering
- Pipe output between programs: ls -l | wc -l 
#### 6) Shell commands configured through arguments/ flags
- ls: list files
- ls -lah: list all files, long listing format, human readable sizes
#### 7) Command Line Environment
- Enviroment Varaibles: set of key-value pairs like HOME=/Users/Wendy
- Examples: $PATH: list of folders where executables are searched for; $CC: default C compiler; $LD_LIBRARY_PATH: search path for the dynamic linker; $TMPDIR: temporary directory
- Environment Variables are not saved, use dotfiles to configurer shell on startup
#### 8）CLI working directory & filesystems:
- All programs have a notion of working directory, the command pwd prints the current working directory that you are "in"
- Relative paths(be resolved with respect to this directory) & Absolute path(do not consider the working directory)
- Directory('.') & Parent Directory('..')
#### 9) $PATH
- Path configures where the shell looks for commands
- The command which can be used to resolve the command. >>which is /bin/ls; >>which python /Users/Wendy/miniconda3/bin/python
- Package managers(e.g. conda) use the path to manage environments. 
- You can also set custom paths if you install additional software. This can be very useful in shared environment(e.g. HPC)
#### 10) CLI dotfiles:
- Hidden by default from "ls" and global expansion
- ~/.bashrc or ~/.zshrc; This file is run at the startup of every shell. Add commands here to make "persistant" changes to the shell environment
- .condarc: This file is used to configure conda(e.g. channels)
- .gitignore: This file is used to exclue files from tracking by git

### Git
- Distributed source control system; keep track of different version of code; not so good for data
- Used through command line tool [GIT](https://missing.csail.mit.edu/2020/version-control/)

### Virtual Environments/Conda
- Manage separate installations of python: Separate packages for different projects; On shared systems: Different installations for different users
- Reproducible environments: Share between collaborators; Ensure that results are stable through time
- Containers: Encapsulate entire system 
- Be careful about system specific packages(especially GPU!) eg: install packages with GPU on cluster machine, but CPU only on your laptop

### Conda
- General purpose package manager: Manage python install; Manage native tools installs(e.g. compilers, CUDA)
- Prefer conda over pip when available: pip has trouble with native dependencies(e.g. CUDA) 
- Avoid installing packages in the base environment
- Try to keep track of packages you have installed. Write an environment.yml file
- For faster installation, use mamba 

### IDE(Integrated Development Environment)
- Code Completion; Debugger; Test Explorer; Notebooks
- VScode(lightweight multi-platform IDE); May also want to consider pycharm; Remote development capabilities: use this extensively.
- [Getting start with VScode](https://code.visualstudio.com/docs/python/python-tutorial)
- [Python Project Sturcutre](https://docs.python-guide.org/writing/structure/)
- setup.py: used to install packages, to install locally, run: pip install -e
- mypackage/: main folder for your package
- tests/: main folder for your tests
- requirement.txt: explicit listing of your dependencies(pip); can also use environment.yml(conda)

### Remote Development
- Leverage powerful hardware: GPUs/accelerators + CPU clusters + Storage for large datasets
- Remote Shells(SSH): Connect a text-based interface to a remote computer; Can be used to tunnel other applications(e.g. file copy, git). Encrypted + Authenticated connection; Username + Password | Key-based authentication(file on local computer) | Hardware key(common in big companies); Can be used to connect other applications(Jupyter | VSCode)
- SSH configuration: SSH is configured through files found in the ~/.ssh folder. ~/.ssh/config file allows for configuration(configure how ssh connects to various hosts, the basic configuration specifies username and host); Public/Private key pairs store in ~./ssh folder; ~/.ssh/id_rsa contains private key
- SSH connection Multiplexing: To avoid the need to reconnect every time, we can multiplex(reuse) one connection. Edit configuration in ~/.ssh/config
- SSH Agent Forwarding: We will often need to further authenticate the remote to further services. eg: laptop->cluster->github; laptop->gateway->cluster; We can ask SSH to forward the authentication through agent forwarding: make sure to add the key to the agent
- SSH ProxyJump: In many large systems, we cannot connect to the remote directly. eg: gateway/ bastion host; NYU HPC: must connect to gw.hpc.nyu.edu from outside VPN; We can ask SSH to proxy the connection through another host using the ProxyJump command

### NYU Green GCP Bursting
- nyugateway(ProxyJump) --> greene(large HPC cluster) --> greeneburst --> burstinstance(GCP on Greene)
- Greene cluster(large HPC cluster): ~550 CPU machines(48 cores each); ~60 GPU machines
- Slurm: System to specify jobs for processing on the cluster; Best suited for batch processing; Used to control allocations of GCP instances
- GCP on Greene: NYU HPC provides a service to access google cloud resources through SLURM; we use this to provision a remote development environment
- Allocate new GCP instance: srun --account=(authorization to allocate the machine) --partition=interactive --time=8:00:00(max 24 hours) --pty /bin/bash(allocate interactive shell); for gpus, we need to specify additionally: --gres=gpu:v100:1
- Check allocated node with: squeue -u USER(run on the log-burst server); Edit the .ssh/config file on laptop to point to the allocated server(Host burstinstance Hostname $log-burst$); Connect with ssh burstinstance

### Remote Development Tips
- Use tmux(terminal multiplexer) to keep the session alive(install tmux through conda); allow multiple panes through a single connection; [cheatsheet](https://gist.github.com/MohamedAlaa/2961058)
- Use htop to monitor CPU/memory usage: (Greene cluster storage: Home:/home/$USER 50GM 30k inodes; Scratch:/scratch/$USER 5TB 1M inodes; Archive:/archive/$USER 2TB 20k inodes; for home/archive, inode limit is severe, don't install conda in home directory!)
- Use nvidia-smi to monitor GPU usage: watch -n1 nvidia-smi(to continuously monitor)

### Greene cluster storage and GCP
- GCP machines see a different filesystem: no "hard" quotas on GCP, also have /home and /scratch but different content
- To transfer data between GCP and Greene, log onto a GCP machine, then use scp to greene-dtn: scp greene-dtn:/scratch/wz2247/data/places365.squashfs

<img width="813" alt="Screen Shot 2022-06-21 at 17 44 56" src="https://user-images.githubusercontent.com/49216429/174902372-c8766734-b36b-4aef-9724-543754bd9ad7.png">


### Singularity
- Container technology(Docker): Built for HPC environment; Can be run without root priviledges; GPU Integration
- Create fully encapsulated portable environments: reproduce your environment between GCP and Greene; share exact environment between team members
- Reuse optimized docker containers: eg: nvidia ngc, tensorflow/ pytorch; Download the Nvidia pytorch docker image; convert it to a singularity image on GCP; run a shell in the image directly: singularity exec /scratch/wz2247/singularity/images/pytorch_21.08-py3.sif /bin/bash
- Bind Paths: allow us to control which paths to expose to the container; (modify files in the main system)
- Overlays: If we want to modify the container. e.g.: install new package in the container environment. /.sif files are immutable; instead, we can use overlays and mount the overlay when starting the container: singularity exec --overlay my_overlay.ext3 /scratch/wz2247/singularity/images/pytorch_21.08-py3.sif bash
<img width="404" alt="Screen Shot 2022-06-21 at 18 13 55" src="https://user-images.githubusercontent.com/49216429/174905789-5ecc1266-cb69-4d13-9dc0-d7bfface781d.png">

























