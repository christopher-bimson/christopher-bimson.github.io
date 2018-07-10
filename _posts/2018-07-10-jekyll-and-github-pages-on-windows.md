---
layout: post
title: Jekyll and GitHub Pages on Windows
---
I've been neglecting blogging again.

I use the combination of [Jekyll](https://jekyllrb.com/) and [GitHub Pages](https://pages.github.com/) for my blog and to make the publishing process smoother I wanted to have the same local testing experience on my Windows machine as I do on my Chromebook.

These are the steps I followed to setup a Ruby environment capable of running Jekyll on a Windows 10 machine.

## Linux Subsystem for Windows

We will use the [Linux Subsystem for Windows](https://blogs.msdn.microsoft.com/wsl/) to run the testing environment.

Several different Linux distributions are available in the Microsoft Store, but these instructions assume the use of Ubuntu. 

1. Open an administrator Powershell prompt and run:

    ```posh
    Enable-WindowsOptionalFeature -Online 
    -FeatureName Microsoft-Windows-Subsystem-Linux
    ```
2. Restart Windows.
3. Open the Microsoft store app and install [Ubuntu.](https://www.microsoft.com/store/productId/9NBLGGH4MSV6) 
4. The first time you run the Ubuntu app you will have to wait a little while for some additional installation steps to complete.
6. Provide a username and password for the Ubuntu environment when prompted.
7. You should now have a working Ubuntu shell.

## Ruby 

We will be using [Ruby Version Manager](https://rvm.io/) to easily install and manage Ruby versions. RVM has a [package available for Ubuntu](https://github.com/rvm/ubuntu_rvm) that makes it nice and easy to install.

1. Ensure that your package lists are up to date by running:

    ```bash
    sudo apt-get update
    ```
    Note that the first time you use `sudo` in a session you will have to provide your password.
2. Install a few prerequisites to ensure that the subsequent steps go smoothly by running:

    ```bash
    sudo apt-get install -y curl gnupg build-essential
    sudo apt-get install software-properties-common
    ```
3. RVM can be installed easily using `apt-get`, but we need to reference the project's personal package archive to do so:

    ```bash
    sudo apt-add-repository -y ppa:rael-gc/rvm
    sudo apt-get update
    ```
4. Install RVM:

    ```bash
    sudo apt-get install rvm
    ```
5. You will need to add your user to the `rvm` group before you can use rvm:

    ```bash
    sudo usermod -a -G rvm <yourUserNameHere>
    ```
6. Restart the Ubuntu app.
7. Install Ruby and Bundler:

    ```bash
    rvm install ruby
    rvm use ruby
    gem install bundler
    ```

## Profit

You now have everything you need to build and run a Jekyll site locally from the Ubuntu shell. If you have an existing  site you can simply navigate to it (you will find your Windows drive at `/mnt/c`) and run:

```bash
 bundle install
 bundle exec jekyll serve
```

If you are starting from scratch you should be able to follow the [instructions](https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/) to create a new GitHub Pages site. 