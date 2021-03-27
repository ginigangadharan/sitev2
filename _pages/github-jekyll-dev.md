
# Install Jekyll & Bundler

(Ubuntu)

Ref: https://jekyllrb.com/docs/installation/ubuntu/

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
