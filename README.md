# Engagement Journal of the Setup Guide: From Installing Rust to Running Docker Containers using Youki as the Default Container Runtimes
- By: (Lakshmi)

![Screenshot from 2024-11-13 23-26-25](https://github.com/user-attachments/assets/8c0d8e4d-4441-43d6-b407-f47def87ffea)

Modified from https://github.com/containers/youki?tab=readme-ov-file to support Ubuntu Linux 22.04 version

## TABLE OF CONTENTS

- [Target To Achieve](#target-of-the-project)
- [Introduction](#introduction-of-the-project)
- [Task 1](#task-1-download-installing--setup-rust--cargo)
- - [Download & Installation](#downloading-and-installation)
- - [Setting up Environment](#setting-up-the-environment-for-the-rust-tool)
- - [Verifying](#verify-the-rust--cargo-installation)
- [Task 2](#task-2-update--download-all-the-dependencies-for-youki)
- - [Updating](#update-the-system)
- - [Downloading and Installing Dependencies](#downloading--installing-dependencies)
- [Task 3](#task-3-git-clonning-adding-repository-installing-just-and-building-youki)
- - [Git Clonning](#git-clonning-repository)
- - [Adding Repo](#adding-the-repository)
- - [Installing Just](#installing-just)
- - [Build Youki](#build-youki)
- - [Make Youki to use](#make-youki-easy-to-use)
- - [Apply Changes](#apply-the-changes)
- - [Verify Youki](#verify-the-youki-installation)
- [Task 4](#task-4-setting-up-youki-as-the-default-container-runtime-for-docker)
- - [Stop Docker Service](#stop-docker-service)
- - [Verify](#verify-that-the-docker-service-stopped)
- - [Edit Docker Configuration File](#editing-docker-configuration-file)
- - [Adding Youki as Default Container Runtimes](#adding-youki-as-a-docker-runtime-in-daemonjson)
- - [Restart Docker Service](#restart-docker-service)
- - [Copying Youki Binary](#copying-youki-binary-to-system-path)
- - [Listing Docker Runtimes](#listing-available-docker-runtimes)
- [Task 5](#task-5-run-a-container-with-youki)
- - [Run a Container](#running-a-container)
- - [Verify](#verify-the-container-is-created-and-running)
- - [Inspecting Container Runtime](#inspecting-container-runtime)
- [Reason & Growth](#reason-and-its-growth)  


## Target of the Project


Build & Deploy a container with the help of Youki Container Runtime.


## Introduction Of the project

Here, I am composing my task-by-task engagement journal about how I am running a container with the help of Youki container runtime. 


## Task-1 Download, Installing & Setup Rust & Cargo

Here, we are downloading & installing Rust(i.e., programming language) & Cargo(i.e., Rust's Package Manager & build system) in which the Youki is built. 

### Downloading and Installation


#### Command

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

#### Need for it

Rust is the programming language in which the youki is also built, and cargo is the package manager & build system for Rust.


#### Error appeared

None

### Setting up the environment for the Rust tool

#### Commands

```bash
source $HOME/.cargo/env 

source ~/.bashrc # Apply the changes immediately to ".bashrc" file for current terminal sessions
```
#### Need for it

It will set up the environment for the Rust tool so that the terminal will find it and can be ready to use by the terminal. Although Rust and Cargo were installed before this step, they still will not be configured until the execution of this step and give zero output to the "--version" command for both(Rust and Cargo).


#### Verify the Rust & Cargo installation

#### Commands

``` bash
rustc --version

cargo --version
```

#### Need for it

Ensuring that Rust and Cargo are installed successfully in the system.


## Task-2 Update & Download all the dependencies for Youki

### Update the system


#### Command

```bash
sudo apt-get update
```

#### Need for it

Update the system with the latest version of all the packages so that it runs smoothly when Youki is installed.


### Downloading & Installing Dependencies

#### Command

```bash
sudo apt-get install    \
    pkg-config          \
    libsystemd-dev      \
    build-essential     \
    libelf-dev          \
    libseccomp-dev      \
    libclang-dev        \
    libssl-dev
```


#### Need for it

These packages, tools, and libraries ensure that from the installation to the working of Youki as a container runtimes correctly.


#### Error appeared
None


## Task-3 Git Cloning, Adding Repository, Installing Just, and Building Youki

### Git Cloning Repository

#### Command

```bash
git clone https://github.com/containers/youki.git
cd youki
```

#### Need for it

This will download the source code from Youki's original repository into the currently made directory of Youki.


#### Error appeared

None


### Adding the Repository

#### Command

```bash
echo "deb [trusted=yes] https://repo.just.systems/apt/ all main" | sudo tee /etc/apt/sources.list.d/just.list
```
#### Need for it

This will add the new software(i.e., "just" which will be installed after it) repository to our system(i.e., prepare the system where to find the just software).


#### Error appeared

None


### Installing Just

#### Command

```bash
cargo install just
```
#### Need for it

This will help set up Youki's development or release mode. 


#### Error appeared

None

### Build Youki

##### Commands

```bash
# Either Build Youki Development 
just youki-dev (# For Testing)


# Or Build Youki Release 
just youki-release (# For Production)

```
##### Need for it

These commands prepare Youki for deployment according to our intended use, enabling either testing or operation in production.

##### Error appeared

None


### Make Youki easy to use

#### Commands

```bash
echo 'export PATH=$PATH:$HOME/youki' >> ~/.bashrc
```
#### Need for it

This will allow us to run Youki from anywhere in our system, which means we don't need to navigate the Youki directory (i.e., made during the cloning part) every time & allow these as permanent changes to ".bashrc".

#### Error appeared

None


### Apply the changes

### Commands

```bash
source ~/.bashrc
```
#### Need for it

This will make the above changes applicable even for the current sessions as well.

#### Error appeared

None


### Verify the Youki Installation

### Command

```bash
youki --version
```
#### Need for it

Ensuring that Youki is being installed successfully in the system.

#### Error appeared

None


## Task-4 Setting up Youki as the Default Container Runtime for Docker

### Stop Docker Service

#### Command

```bash
sudo systemctl stop docker
```
#### Need for it

Here, we have to stop docker services in order to make changes in the docker daemon file.

#### Error appeared

None


### Verify that the Docker service stopped

#### Command

```bash
sudo systemctl status docker
```
#### Need for it

Here, we ensure that the Docker service stops in order to make changes in the Docker daemon file.

#### Error appeared

None


### Editing Docker Configuration File

#### Command

```bash
sudo nano /etc/docker/daemon.json
```
#### Need for it

This will open the docker daemon.json file to edit out.

#### Error appeared

None


### Adding Youki as a Docker Runtime in daemon.json

#### Command

```bash

{
  "runtimes": {
    "youki": {
      "path": "/usr/local/bin/youki"
    }
  },
  "default-runtime": "youki"
}

```
#### Need for it

This will set Youki as the default container runtime for the Docker(i.e., originally runc).

#### Error appeared

None


### Restart Docker Service

#### Command

```bash
sudo systemctl start docker
```
#### Need for it

This will restart docker services after setting up Youki as the default container runtime for the docker.

#### Error appeared

None


### Verifying Docker Service Restart

#### Command

```bash
sudo systemctl status docker
```
#### Need for it

This will verify that the docker service is restarted after setting up Youki as the default container runtime for the docker.

#### Error appeared

None


### Copying Youki Binary to System Path

#### Command

```bash
sudo cp ~/youki/youki /usr/local/bin/ 
```
#### Need for it

This will copy the Youki binary to "/usr/local/bin" so it can be accessed by the system command and by Docker as well.

#### Error appeared

None


### Making Youki Executable

#### Command

```bash
sudo chmod +x /usr/local/bin/youki
```
#### Need for it

This will grant executable permissions to Youki, ensuring it can run as a command and will also be used by docker as its runtime.

#### Error appeared

None


### Listing Available Docker Runtimes

#### Command

```bash
docker info | grep "Runtimes"
```
#### Need for it

This will display all the Docker runtimes present in our system. If it displays Youki as well then it means Youki is also the container runtime for docker as well.

#### Error appeared

None


## Task-5 Run a container with Youki

### Running a container

#### Command

```bash
docker run --rm --runtime=youki -it alpine
```
#### Need for it

This will run the container using the Alpine image with the help of Youki Container runtime.

#### Error appeared

None

### Verify the Container is Created and Running

Without exiting the container open one adjacent new terminal tab

#### Command

```bash
docker ps -a 
```
#### Need for it

This will verify that the container is made successfully.

#### Error appeared

None


### Inspecting Container Runtime

Run it on the same terminal tab used above.

#### Command

```bash
docker inspect 25e32bbc05dc | grep Runtime 
```
#### Need for it

This will verify that the recently made container is using Youki as its runtime.

#### Error appeared

None


## Reason and its Growth

Youki is a great alternative to traditional container runtimes(i.e., runc), as it is built using Rust (Modern programming language) known for its safety and fast performance with low memory usage compared to runc, as is still in the developing stage. 
Runc has been the default runtime for Docker and containerd for years, making it handy for container setups. 
As we see [Benchmark Example](https://github.com/user-attachments/assets/8c0d8e4d-4441-43d6-b407-f47def87ffea)(i.e., at the "Motivation" section) comparing Youki, runc, and another runtime, crun, shows Youki performing operations in 111.5ms, while runc takes 224.6ms. This portrays that Youki is roughly 50% faster in specific tasks.
