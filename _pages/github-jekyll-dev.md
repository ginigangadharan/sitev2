---
layout: post
title: How to Configure and Use GitHub Jekyll Local Environment
author: gini
categories: [ jekyll ]
image: "assets/images/2020/infrastructure.jpg"
tags: [web, jekyll]
permalink: jekyll-local
featured: false
hidden: false
titleshort: jekyll
---
# Install Jekyll & Bundler

Refer the documentation based on your Operating System.

[Ubuntu](https://jekyllrb.com/docs/installation/ubuntu/) | [MacOS](https://jekyllrb.com/docs/installation/macos/) | [Linux](https://jekyllrb.com/docs/installation/other-linux/) | [Windows](https://jekyllrb.com/docs/installation/windows/)

# Instructions for MacOS

## Install Command Line Tools

```shell
xcode-select --install
```

set SDKROOT (only macOS Catalina or later)Permalink

```shell
export SDKROOT=$(xcrun --show-sdk-path)
```

## Install Ruby

Jekyll requires Ruby v2.5.0 or higher but MacOS Big Sur 11.x is already installed with Ruby 2.6.3. Check the version and confirm.

```shell
% ruby -v
ruby 3.0.2p107 (2021-07-07 revision 0db68f0233) [x86_64-darwin20]
```

If you’re running a previous version of macOS, you’ll have to install a newer version of Ruby. 

## Install Jekyll

(Use localinstall rathr than Global Install)

```shell
gem install --user-install bundler jekyll
```

Append your path file with the following, replacing the X.X with the first two digits of your Ruby version:

```shell
# If you're using Zsh
echo 'export PATH="$HOME/.gem/ruby/X.X.0/bin:$PATH"' >> ~/.zshrc

# If you're using Bash
echo 'export PATH="$HOME/.gem/ruby/X.X.0/bin:$PATH"' >> ~/.bash_profile

# Unsure which shell you are using? Type
echo $SHELL
```

Check that GEM PATHS: points to your home directory:

```shell
gem env
```

# Instructions for Ubuntu
## Install packages


```bash
## Ubuntu
sudo apt-get install ruby-full build-essential zlib1g-dev

## RHEL8/CentOS8
sudo dnf install ruby ruby-devel
sudo dnf group install "Development Tools"
```


## Configure gem path
```
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

## Install jekyll & bundler
```
gem install jekyll bundler
```

# Test your site
Goto your project directory
```
bundle exec jekyll serve
```

# Updating the GitHub Pages gem
If the github-pages gem on your computer is out of date with the github-pages gem on the GitHub Pages server, your site may look different when built locally than when published on GitHub. To avoid this, regularly update the github-pages gem on your computer.

- If you installed Bundler, run bundle update github-pages.
- If you don't have Bundler installed, run gem update github-pages.


# If no Gemfile

```
bundle init
bundle add jekyll
bundle install --path vendor/bundle
```
