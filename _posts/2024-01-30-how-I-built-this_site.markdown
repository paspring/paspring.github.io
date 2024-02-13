---
layout: single
title: Building My Jekyll Site with Docker and Minimal Mistakes
author_profile: true
toc: true
---
In this post, I'd like to share the process I followed to build this website. These notes are primarily for personal reference, but if you're interested in recreating what I've done here, feel free to follow along.

## Docker Setup
The first step in this process is to set up a Docker image with the proper requirements. Using Docker instead of your local machine for deployment offers several advantages, such as:

- Isolated environment: Ensures consistency across different machines.
- Easy setup: Simplifies the process of configuring development environments.
- Reproducibility: Makes it easy to replicate the setup on other machines.

Here's the Dockerfile I used:

```Dockerfile
FROM ubuntu:22.04
RUN apt-get update
RUN apt-get -y install git curl autoconf bison build-essential libssl-dev 
    libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev 
    libgdbm6 libgdbm-dev libdb-dev apt-utils

ENV RBENV_ROOT /usr/local/src/rbenv
ENV RUBY_VERSION 3.1.2
ENV PATH ${RBENV_ROOT}/bin:${RBENV_ROOT}/shims:$PATH
ENV LC_ALL C.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8

RUN git clone https://github.com/rbenv/rbenv.git ${RBENV_ROOT} \
  && git clone https://github.com/rbenv/ruby-build.git ${RBENV_ROOT}/plugins/ruby-build \
  && ${RBENV_ROOT}/plugins/ruby-build/install.sh \
  && echo 'eval "$(rbenv init -)"' >> /etc/profile.d/rbenv.sh

RUN rbenv install ${RUBY_VERSION} \
  && rbenv global ${RUBY_VERSION}

RUN gem install jekyll -v '3.9.3'
```

## VSCode Setup
You'll need Visual Studio Code (VSCode) with the Docker and Dev Containers extensions, as well as Docker Desktop running on your system. Once everything is ready, start your dev container and create the image from the Dockerfile inside your project's repository.

## Install Jekyll Site
With the Docker environment set up, create a new Jekyll file structure inside the repo by running:

```bash
jekyll new . --skip-bundle --force
```

## Install Your Theme
I chose the Minimal Mistakes theme. Depending on your theme choice, this step may differ. For Minimal Mistakes, modify the `Gemfile` and `_config.yml` as follows:

In the `Gemfile`, replace the current content with:

```ruby
source "https://rubygems.org"

gem "github-pages", group: :jekyll_plugins
gem "jekyll-include-cache", group: :jekyll_plugins

gem "webrick"
```

In `_config.yml`, make these changes:
- Change the `theme` line to `remote_theme: "mmistakes/minimal-mistakes@4.24.0"`.
- Modify the `plugins` as follows:

  ```yaml
  plugins:
    - jekyll-feed
    - jekyll-include-cache
  ```

Finally, after saving all changes, install the bundle requirements by running:

```bash
bundle install
```

## Start Your Website Locally
To run your website locally, use the following command:

```bash
bundle exec jekyll serve --livereload
```

## Customization
You can find ways to customize your website in the [Minimal Mistakes documentation](https://mmistakes.github.io/minimal-mistakes/).






