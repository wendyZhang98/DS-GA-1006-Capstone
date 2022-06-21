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
1) Text-based interface for computer; Access through terminal
- Programmable/ Automatable
- Translate well across platforms(Laptop, Desktop, HPC Cluster, Cloud)
2) Shell Basics
- Shell is a programming language
- Main programming aspects we make use of: variables, redirection, globbing
3) Shell Variables
- All variables in Shell are "strings"
- Define a variable by assignment: MY_VAR="my value"
- Access variable value by expansions(string replacement): echo"$HOME/$MY_VAR"
- Double quotes(") enalble expansions, but single quotes(') don't!: echo'$MY_VAR' doesn't work
- Pass variables to prgram: export, otherwise value only set in your shell
4) Shell Globbing
- *: match any number of characters: echo *.txt lists all text files 
- ?: match single character
- [1-9]: match any single digit
5) Shell Redirection
- Programs can communicate with each other through standard pipes: standard output=1, standard error=2
- Redirect output to files: echo "Test" data 1>output.txt (redirect standard output to output.txt); python -m script.py 2>&1 (redirect standard error to standard output)
- Be careful about buffering: python -u *.py will run without buffering
- Pipe output between programs: ls -l | wc -l 
6) Shell commands configured through arguments/ flags
- ls: list files
- ls -lah: list all files, long listing format, human readable sizes
7) Command Line Environment
- Enviroment Varaibles: set of key-value pairs like HOME=/Users/Wendy
- Examples: $PATH: list of folders where executables are searched for; $CC: default C compiler; $LD_LIBRARY_PATH: search path for the dynamic linker; $TMPDIR: temporary directory
- Environment Variables are not saved, use dotfiles to configurer shell on startup
8）CLI working directory & filesystems:
- All programs have a notion of working directory, the command pwd prints the current working directory that you are "in"
- Relative paths(be resolved with respect to this directory) & Absolute path(do not consider the working directory)
- Directory('.') & Parent Directory('..')
9) $PATH
- Path configures where the shell looks for commands
- The command which can be used to resolve the command. >>which is /bin/ls; >>which python /Users/Wendy/miniconda3/bin/python
- Package managers(e.g. conda) use the path to manage environments. 
- You can also set custom paths if you install additional software. This can be very useful in shared environment(e.g. HPC)
10) CLI dotfiles:
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






