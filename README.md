# DS-GA-1006-Capstone

Goals of the Bootcamp
- Workflow: Github, Collaboration, Testing, Reproducibility
- Software tools: IDE, CLI, Environment Management(Docker/Singularity)
- System Knowledge: HPC, GPUs

Data Science Software Stack
- CLI
- IDE(Vscode), Debugging, Testing, Notebook
- Python(Conda), Programming with GPUs

[Resource Covering a lot of the general software engineering material in more detail](https://missing.csail.mit.edu/)

[Practical Examples given by WendaZhou](https://github.com/wendazhou/cds-bootcamp)

Shell: 
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
6) Shell commands
- ls: list files
- ls -lah: list all files, long listing format, human readable sizes
- 

