# Systems Integrator Skills Evaluator
This provides a simple set of exercises under the [Ignition](http://www.inductiveautomation.com) platform to establish a baseline capability for folks wanting to enter the industrial automation space as a Systems Integrator (SI).

## Prerequisites

* VirtualBox
* Vagrant

## Getting Started

You'll need to have a system with ideally 6GB of system RAM in order to spool up the evaluator.  If you don't have a system available with enough resources, touch base with your ENE representative.

We're using [Vagrant](http://www.vagrantup.com) to handle the deployment of the environment.  Make sure that you have Vagrant installed by getting a download here:

https://www.vagrantup.com/downloads.html

You also need to have VirtualBox installed as the VM provider.  Get VirtualBox here:

https://www.virtualbox.org/wiki/Downloads

### Windows

First, download the repository to your computer (if you have `git` installed, you can clone as well):
> *Optional*: If you don't have a `git` client and want to be able to work with it, install *Git for Windows* 
> and get not only `git` but also `ssh` (and more):  [Git for Windows](https://git-for-windows.github.io)

![Download from Github](images/download_from_github.png)

Unpack the downloaded zip file, open a command prompt to the resulting location, and run `vagrant up`:

![Launching Vagrant](images/launching_vagrant.png)

### Linux / macOS

First, open a terminal and clone this repository to your computer:

```bash
git clone https://github.com/kcollins-ene/evaluator.git
```

> If you don't have `git` installed, you may get some additional output during this step.  macOS will typically prompt you to install the Xcode developer tools—once completed, you may need to re-run the `git` command above.

Next, start the environment:

```shell
cd evaluator
vagrant up
```


### All

Following the platform-specific steps above will create and provision an Ubuntu 16.04 development environment with Oracle Java8, MySQL, and Ignition 7.9.3 preinstalled.  Next, simply launch a web browser against the forwarded port on your local computer (note that if for some reason there was a port collision, you might need to enter a port other than 8088 here, consult the output from `vagrant up`):

http://localhost:8088

If you see an Ignition Gateway Webpage, you're ready to go:

![Ignition Home Page](images/ignition_home_page.png)

## Performing the evaluation

With your development environment online and ready, proceed to the [Evaluation](Evaluation.md) section for more instructions!

## Shutting down the environment

In order to shutdown the environment, run the following command in the `evaluator` folder:

```shell
vagrant halt
```

This will shutdown the virtual machine and release the network port configuration that was setup on launch.

[^1]: VirtualBox 5.1.16 Regression: https://www.virtualbox.org/ticket/14651

[^2]: Vagrant Shared Folder / Extended Path Issue: https://github.com/mitchellh/vagrant/issues/8352
