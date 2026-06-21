
In this interactive tutorial, you'll learn how to navigate the Linux filesystem and use fundamental bash commands through detailed, hands-on examples.

## Why Learn Linux for Bioinformatics?

Linux is essential in bioinformatics and computational biology for several reasons:

- Most bioinformatics tools are developed for Linux environments.
- Linux enables processing of large genomic datasets efficiently.
- Command-line operations can be automated to handle repetitive tasks.
- High-performance computing clusters, such as IBEX, run on Linux.

As you progress in infectious disease research, these skills will help you analyze sequencing data, run phylogenetic analyses, and process large datasets more effectively.

## Getting Started with the Tutorial

It is best to manually type each command into the terminal. Make sure to include spaces exactly as shown, because they are critical for commands to work correctly. Although copying and pasting may seem quicker, typing commands yourself helps you become more comfortable with the command line and supports memorization of the basics.

In this tutorial, comments in the code are marked with `#`. These comment lines show the generic form of a command and are not executed by the operating system.

```bash
# This is a comment line showing the generic form of the command.
# The actual command you need to type and execute follows the comment.
```

## Linux Basics

### Understanding the Linux File System Structure

The Linux filesystem is organized like a tree. Unlike Windows, it does not use drive letters such as `C:`. The root of the filesystem is denoted by `/`, which represents the top level of the directory tree.

### Understanding the Home Directory

When you open your terminal, you begin in your home directory. To determine its location, use the `pwd` command, which stands for "print working directory".

```bash
pwd
```

Typical output:

```text
/home/username
```

## Essential File Operations

### Accessing Tutorial Files

Start by downloading the tutorial materials:

```bash
git clone https://github.com/gzhoubioinf/IDE_LinuxTutorial.git
```

After the download completes, check the contents of your home directory with `ls`. You should see a directory named `IDE_LinuxTutorial`.

### Navigating and Inspecting Directory Contents

Navigate into the new directory:

```bash
cd IDE_LinuxTutorial
```

List the contents:

```bash
ls
```

Example output:

```text
fasta_files  README.md  sequence.gb
```

A more informative listing uses options:

```bash
# Command with options
ls -lht
```

Example output:

```text
-rw-r--r-- 1 username Users 11M Feb 2 07:30 sequence.gb
-rw-r--r-- 1 username Users 39B Feb 2 07:30 README.md
drwxr-xr-x 2 username Users 192B Feb 2 07:30 fasta_files
```

Explanation of the options:

- `-l` shows a detailed listing format.
- `-h` shows file sizes in human-readable units such as `K`, `M`, or `G`.
- `-t` sorts items by modification time.

### Understanding File Permissions

When you see `-rw-r--r--` in `ls -l` output, this represents file permissions:

- First character: file type (`-` for file, `d` for directory)
- Next three characters: owner permissions
- Next three characters: group permissions
- Final three characters: permissions for everyone else

Examples:

- `-rw-r--r--`: owner can read and write; others can only read
- `drwxr-xr-x`: directory where owner can read, write, and execute; others can read and execute

To read the manual page for `ls`:

```bash
man ls
```

## Exploring File Contents

A good place to start is the `README.md` file:

```bash
more README.md
```

To explore a large file incrementally:

```bash
more sequence.gb
```

Example content:

```text
LOCUS       JAN-2025
DEFINITION  chromosome,
ACCESSION   NZ_CP097881
...
Escherichia coli str. K-12 substr. MG1655 strain K-12
complete genome.
```

To display the whole file directly:

```bash
cat sequence.gb
```

While using `more`, press `Enter` to advance through the file and `q` to quit.

To preview the beginning and end of a file:

```bash
head sequence.gb
tail sequence.gb
```

To view more than the default 10 lines at the start:

```bash
head -n 15 sequence.gb
```

## File Manipulation (Copy, Move, Rename)

Create a temporary folder and move into it:

```bash
mkdir tmp
cd tmp
```

Copy a file:

```bash
cp ../sequence.gb .
ls
```

Example output:

```text
sequence.gb
```

To remove a file:

```bash
rm sequence.gb
```

Navigate to the parent directory, rename a GenBank file, and view it:

```bash
cd ../
mv sequence.gb sequence_1.gb
more sequence_1.gb
```

Move the GenBank file into a temporary directory, list its contents, then move the file back:

```bash
mv sequence_1.gb ./tmp/
ls tmp/
mv tmp/sequence_1.gb ./
ls
```

## Navigation and Path Concepts

### Absolute vs Relative Paths

There are two main ways to specify locations in Linux: absolute paths and relative paths.

#### File Structure Visualization

```text
/Users/username/IDE_LinuxTutorial/
└├── README.md
├── fasta_files
│   ├── README.md
│   ├── frog.fasta
│   ├── human.fasta
│   └── mouse.fasta
├── sequence_1.gb
└── tmp
    └── sequence.gb
```

#### Absolute Paths

An absolute path starts from the root directory `/` and gives the complete location of a file or directory.

```bash
# Your home directory (absolute path)
/Users/username/IDE_LinuxTutorial
```

Examples:

```bash
# Navigate back to your home directory
cd ~

# Navigate directly using an absolute path
cd /Users/username/IDE_LinuxTutorial/tmp
```

#### Relative Paths

A relative path specifies a location relative to your current working directory.

```bash
# Navigate to a subdirectory using a relative path
cd fasta_files
```

Example of an invalid relative path from inside `fasta_files`:

```bash
cd tmp
# Error: bash: cd: tmp: No such file or directory
```

This happens because `fasta_files` and `tmp` are sibling directories.

#### Path Navigation Symbols

- `.` = current directory
- `..` = parent directory
- `~` = home directory

To correctly navigate from `fasta_files` to `tmp`:

```bash
# First go up one level
cd ..

# Then go down into tmp
cd tmp

# Or combine both steps
cd ../tmp
```

### Directory Navigation Shortcuts

```bash
# Go to home directory
cd ~

# Move up one directory level
cd ..

# Move up multiple levels
cd ../../

# Go to previous directory
cd -

# Go to specific absolute path
cd /path/to/directory

# Go to relative path
cd folder/subfolder
```

## Creating and Deleting Content

Create a new file:

```bash
# Create a new file
touch mynewfile
```

Append text to a file, view it, then overwrite it:

```bash
echo "Hello World" >> mynewfile
more mynewfile

echo "My name is Danesh" >> mynewfile
more mynewfile

echo "My name is Danesh" > mynewfile
more mynewfile
```

## Searching for Content

Find files by name:

```bash
touch mynewfile1
find . -name "mynewfile"
find . -name "my*"
find . -name "f*"
find . -name "*f*"
```

The period `.` means the current working directory.

If you want to find all FASTA files, use a wildcard:

```bash
find . -name "*.fasta"
```

Example output:

```text
./fasta_files/frog.fasta
./fasta_files/human.fasta
./fasta_files/mouse.fasta
```

## Working with File Content

### Viewing Files (`more`, `less`, `cat`)

Basic viewing with `more`:

```bash
more sequence.gb
```

Viewing with `less`:

```bash
less sequence.gb
```

Useful keys in `less`:

- `spacebar` to move forward
- `b` to move backward
- `q` to quit

Display the full contents of a file with `cat`:

```bash
cat sequence.gb
```

You can also combine files:

```bash
# First create and write to the individual files
echo "Content for file 1" > file1.txt
echo "Content for file 2" > file2.txt

# Then combine them
cat file1.txt file2.txt > combined.txt
```

## Exercise

Loop from 0 to 50 and print each number:

```bash
for i in {0..50}; do echo ${i}; done
```

Use a `for` loop to generate multiple files and write indexed content into each one:

```bash
for i in {0..50}; do
  touch file_${i}.txt
  echo "My ID is ${i}" > file_${i}.txt
 done

more file_14.txt
cat file_* > tot_file.txt
less tot_file.txt
sort -V tot_file.txt
```

## Analyzing File Content (`wc`, `grep`)

Count lines, words, and characters in a file:

```bash
wc sequence.gb
```

Example output:

```text
176336 741597 11539404 sequence.gb
```

This means:

- Lines: `176336`
- Words: `741597`
- Characters: `11539404`

To count only lines:

```bash
wc -l sequence.gb
```

To count only words:

```bash
wc -w sequence.gb
```

Search within a file using `grep`:

```bash
grep "chromosome" sequence.gb
```

## Manipulating File Content (`sort`, `uniq`)

### Using `sort` to Organize Data

```bash
# Create the file first
touch names.txt

# Add content
echo "Charlie" > names.txt
echo "Alice" >> names.txt
echo "Bob" >> names.txt

# Sort lines in a file alphabetically
sort names.txt
```

Sort numbers numerically:

```bash
# Create the file first
touch numbers.txt

# Add some numbers
echo "10" > numbers.txt
echo "2" >> numbers.txt
echo "1" >> numbers.txt

# Sort numerically
sort -n numbers.txt
```

`-n` tells `sort` to treat the input as numbers instead of strings.

Sort in reverse order:

```bash
sort -r names.txt
```

### Using `uniq` to Find Unique Entries

```bash
# Show unique lines from a sorted file
sort names.txt | uniq
```

Important: `uniq` removes only consecutive duplicates, so you usually sort first.

Count occurrences of each unique line:

```bash
echo "Alice" > names.txt
echo "Bob" >> names.txt
echo "Charlie" >> names.txt
echo "Alice" >> names.txt
echo "Charlie" >> names.txt
echo "Charlie" >> names.txt
sort names.txt | uniq -c
```

Example output:

```text
2 Alice
1 Bob
3 Charlie
```

## Text Editors

Linux offers a variety of text editors. This tutorial covers two common ones: `nano` and `vim`.

### Nano - Beginner-Friendly Editor

Open Nano:

```bash
nano filename.txt
```

Basic commands:

- `CTRL+O`: save file
- `CTRL+X`: exit the editor
- `CTRL+K`: cut the current line
- `CTRL+U`: paste the cut text

### Vim - Advanced Text Editor

Open Vim:

```bash
vim filename.txt
```

Modes:

- **Normal mode**: navigate and perform editing commands
- **Insert mode**: type and edit text (`i`)
- **Command mode**: save or quit (`:`)

Basic commands:

- `:w` save the file
- `:q` quit Vim
- `:wq` save and quit
- `i` switch to insert mode
- `ESC` return to normal mode

Navigation in normal mode:

- `h` left
- `j` down
- `k` up
- `l` right

## Shell Scripting Basics

This section introduces a simple Bash workflow that creates files and combines them.

### Step 1: Create the Script File

```bash
touch pipeline.sh
```

### Step 2: Write the Script

```bash
vim pipeline.sh
```

Script contents:

```bash
mkdir -p test
cd test
for i in {1..100}; do
  touch file_${i}.txt
  echo "My ID is ${i}" > file_${i}.txt
 done
cat file_* > tot_file.txt
```

### Making Scripts Executable

```bash
chmod +x pipeline.sh
```

### Running the Script

```bash
./pipeline.sh
```

## Advanced Topics: HPC Environment

### HPC Login and Basic Commands

High-performance computing (HPC) clusters such as IBEX provide computational power far beyond desktop computers. These systems are essential for processing large genomic datasets.

### Connecting to IBEX

```bash
# Connect to IBEX using SSH with X11 forwarding
ssh -X username@ilogin.ibex.kaust.edu.sa
```

### The Module System

IBEX uses modules to manage software environments and allow multiple versions of software to coexist.

```bash
# List all available software
module avail

# Search for specific software
module avail python

# Load a specific version
module load python/3.11.0

# Check currently loaded modules
module list
```

## SLURM Job Submission

SLURM manages job scheduling on HPC systems.

Go to the working directory and create a job script:

```bash
cd /ibex/project/c2325/B200/Day2_Intro_Linux/assembly_test
vim amrfinder.sh
```

Sample SLURM script:

```bash
#!/bin/bash
#SBATCH --nodes=1
#SBATCH --cpus-per-task=8
#SBATCH --job-name=amrfinder
#SBATCH --error=myjob.%J.err
#SBATCH --time=2:00:00

amrfinder --nucleotide ${1} -O Acinetobacter_baumannii -o output_${1}.txt
```

Give execute permission and test locally:

```bash
chmod +x amrfinder.sh
./amrfinder.sh assembly_N1002.fasta
```

Submit one job per FASTA file:

```bash
for i in assembly_N*.fasta; do
  sbatch amrfinder.sh "$i"
done
```

Check the queue and view output:

```bash
squeue --me
less output_assembly_N1059.fasta.txt
```

### SLURM Parameters Explained

| Parameter | Description | Example |
|---|---|---|
| `--job-name` | Name displayed in the job queue for identification | `--job-name=genome_assembly` |
| `--nodes` | Number of compute nodes to allocate for the job | `--nodes=1` |
| `--ntasks` | Total number of processes to run | `--ntasks=1` |
| `--cpus-per-task` | CPU cores allocated to each task | `--cpus-per-task=16` |
| `--mem` | Total memory allocation per node | `--mem=32G` |
| `--time` | Maximum job runtime in `D-HH:MM:SS` format | `--time=12:00:00` |
| `--output` | File path for standard output (`stdout`) | `--output=assembly_%j.out` |
| `--error` | File path for error output (`stderr`) | `--error=assembly_%j.err` |
| `--mail-user` | Email address for job notifications | `--mail-user=username@example.com` |
| `--mail-type` | When to send email notifications | `--mail-type=BEGIN,END,FAIL` |
| `--partition` | Specific queue or partition for the job | `--partition=batch` |
| `--account` | Project account to charge | `--account=bioinformatics` |
| `--gres` | Special resources such as GPUs | `--gres=gpu:1` |

### Submitting and Managing Jobs

```bash
# Submit a job
sbatch myjob.slurm

# Check status of all your jobs
squeue -u $USER

# Check detailed job info
scontrol show job JOB_ID

# Cancel a job
scancel JOB_ID

# Cancel all your jobs
scancel -u $USER

# Submit to a specific partition
sbatch --partition=batch myjob.slurm
```

## File Transfer Between Systems

Transferring data to and from HPC systems is a common task in bioinformatics workflows.

### Using SCP

```bash
# Copy local file to IBEX
scp sequence.fasta username@ilogin.ibex.kaust.edu.sa:~/projects/

# Copy directory to IBEX recursively
scp -r local_directory username@ilogin.ibex.kaust.edu.sa:~/projects/

# Copy from IBEX to local machine
scp username@ilogin.ibex.kaust.edu.sa:~/results/analysis.txt ./
```

### Using Rsync

```bash
# Sync local directory to IBEX
rsync -avz --progress local_data/ username@ilogin.ibex.kaust.edu.sa:~/projects/
```

## Troubleshooting and References

### Common Errors for Beginners

| Error Message | Likely Cause | Solution |
|---|---|---|
| `Permission denied` | File lacks execute permission | Use `chmod +x filename` |
| `Command not found` | Program not installed or not in `PATH` | Check spelling or install with a package manager |
| `No such file or directory` | Incorrect path or filename | Use `ls` to verify the file exists and check spelling |
| `Is a directory` | A file-oriented command was used on a directory | Specify a file instead of a directory |

### Time-Saving Keyboard Shortcuts

| Shortcut | Function |
|---|---|
| `Tab` | Auto-complete commands and filenames |
| `Up arrow` | Recall previous commands |
| `Ctrl+C` | Interrupt or cancel current command |
| `Ctrl+Z` | Suspend current process |
| `Ctrl+L` | Clear the terminal screen |
| `Ctrl+A` | Move cursor to beginning of line |
| `Ctrl+E` | Move cursor to end of line |

### Summary of Key Commands

| Command | Function |
|---|---|
| `cd` | Change directory |
| `ls` | List directory contents |
| `pwd` | Show current directory |
| `cp` | Copy files |
| `mv` | Move or rename files |
| `rm` | Remove files |
| `mkdir` | Make a new directory |
| `touch` | Create a new empty file |
| `find` | Search for files by name |
| `grep` | Search for content inside files |
| `nano` | Basic text editor |
| `vim` | Advanced text editor |
| `module` | Manage software environments on IBEX |
| `sbatch` | Submit SLURM job script |
| `sinfo` | Show available nodes |
| `squeue` | Show current jobs |
| `scancel` | Cancel a job using its job ID |

## Further Learning Resources

### Official Documentation and Reference Materials

- [GNU/Linux Command-Line Tools Summary](https://tldp.org/LDP/GNU-Linux-Tools-Summary/html/index.html) - Comprehensive guide to command-line tools
- [Bash Reference Manual](https://www.gnu.org/software/bash/manual/bash.html) - Official Bash documentation

### Cheat Sheets

- [Linux Command Line Cheat Sheet](https://cheatography.com/davechild/cheat-sheets/linux-command-line/) - Quick reference
- [Vim Cheat Sheet](https://vim.rtorr.com) - Essential Vim commands
- [Bash Scripting Cheat Sheet](https://devhints.io/bash) - Quick bash reference
