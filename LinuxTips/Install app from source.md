

## Step 1: Installing the Required Tools

Linux provides you with all the necessary tools required to compile, build, and install software from the source code.

Most Linux software is written in the C or C++ programming languages, therefore, you'll need a C or C++ compiler. For example, the GNU Compiler Collection (GCC) and CMake for building your package.

Besides that, you'll need other packages such as curl and gettext. Depending on your Linux distro, you can install the required tools in a single command as follows.
On Debian-based distros such as Ubuntu:

 `sudo apt install libz-dev libssl-dev libcurl4-gnutls-dev libexpat1-dev gettext cmake gcc curl` 

On Arch Linux and its derivatives:

 `sudo pacman -S base-devel` 

On RPM-based distros such as Fedora, RHEL, etc:

 `sudo dnf install dh-autoreconf curl-devel expat-devel gettext-devel openssl-devel perl-devel zlib-devel gcc curl cmake`

## Step 2: Downloading the Package Source Code

```
curl --output git.tar.gz https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.26.2.tar.gz
```

To [extract the content of the zipped file](https://www.makeuseof.com/extract-tar-gz/), you can use the **tar** command.

 `tar -zxf git.tar.gz`
 
## Build system explain

It is always a good idea to take a look at the **README.md** or **INSTALL** files because they contain valuable information on how to compile and install the package. These files are usually located in the root folder of the source code.

Another important file is the **configure** script. It checks for software dependencies for the package you want to compile, and you'll see an error message if the script finds missing dependencies.
Writing and tuning a build system is a pretty complex task, but for the “end user”, GNU-style build systems ease the task by using two tools: `configure` and `make`.

The `configure` file is a project-specific script that will check the destination system configuration and available feature in order to ensure the project can be built, eventually dealing with the specificities of the current platform.

An important part of a typical `configure` job is to build the `Makefile`. That is the file containing the instructions required to effectively build the project.

The [`make` tool](https://en.wikipedia.org/wiki/Make_(software)?ref=itsfoss.com), on the other hand, is a POSIX tool available on any Unix-like system. It will read the project-specific `Makefile` and perform the required operations to build and install your program.


## Compiling source code
Next, go to the newly extracted folder. In this case, the name will be "git-2.26.2," of course, the folder name will be different if you have downloaded a different version of Git.

 `cd git-2.26.2` 

Configure and prepare your source code by executing the script. The command will create **make** files and configurations for the software that you are about to compile and install.

 `./configure`


Now that the source code is configured and compiled, you can build the software as follows:

 `make`