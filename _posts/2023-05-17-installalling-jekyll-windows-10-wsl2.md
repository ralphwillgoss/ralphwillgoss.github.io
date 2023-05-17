---
title: "Installing Jekyll on Windows 10 using WSL2"
date: 2023-05-17
categories:
  - wsl2 jekyll windows10
tags:
  - wsl2 jekyll windows10
---
I recently setup my static site generator [Jekyll](https://jekyllrb.com/) on a new PC, in order to maintain this blog.  
As part of this, I decided to use [Windows Subsystem for Linux v2 (WSL2)](https://learn.microsoft.com/en-us/windows/wsl) for my installation.  

The goal of this was to simplify the [Ruby]() setup, which is a dependency of Jekyll.  
I followed the Jeykyll [Windows installation instructions](https://jekyllrb.com/docs/installation/windows/).  

However, this did not work as expected and below is a guide to the modifications I had to do and some errors I encountered.

# Versions 
The following is a list of core components I am using and their versions:
```shell
- Microsoft Windows 10 Pro (10.0.19045 Build 19045)
- Ubuntu-20.04
- WSL2: 1.2.5.0 (Kernel version: 5.15.90.1)
- Jekyll: 4.3.2
- Bundler: 2.4.12
```

# Installation
The following is a list of changes I needed to make, while following the Jekyll installation guide.
- Updated WSL2: `wsl --update`
- Had to run most commands with `sudo` (most likely due to my minimalist setup of `wsl`)
- Install Ruby 2.6 (not 2.5 as per instructions)
  - `sudo apt-get install ruby2.6 ruby2.6-dev build-essential dh-autoreconf`


# Trouble Shooting
Below are some of the commands I executed, the errors I received, and the solutions to fixing them.

### File System watching no longer works
If you have upgraded from WSL1 to WSL2, auto rebuilding of the site using the `--watch` flag no longer works.  
This is a known issue as reported on [github](https://github.com/microsoft/WSL/issues/216).

**Solution:**  
The use of the `--force_polling` flag gets around this issue:  
`bundle exec jekyll serve --force_polling`

Some people reported additional load on their machines when doing this, so far I've not noticed any bad side affects.

### There are no versions of xxxxx (= x.x.x) compatible with your Ruby & RubyGems
Command: `sudo gem update`  

```shell
ERROR:  Error installing date:
        There are no versions of date (= 3.3.3) compatible with your Ruby & RubyGems
        date requires Ruby version >= 2.6.0. The current ruby version is 2.5.0.
		  
ERROR:  Error installing etc:
        There are no versions of etc (= 1.4.2) compatible with your Ruby & RubyGems
        etc requires Ruby version >= 2.6.0. The current ruby version is 2.5.0.
		  
ERROR:  Error installing io-console:
        There are no versions of io-console (= 0.6.0) compatible with your Ruby & RubyGems
        io-console requires Ruby version >= 2.6.0. The current ruby version is 2.5.0.

ERROR:  Error installing minitest:
        There are no versions of minitest (= 5.18.0) compatible with your Ruby & RubyGems
        minitest requires Ruby version < 4.0, >= 2.6. The current ruby version is 2.5.0
		  
ERROR:  Error installing openssl:
        There are no versions of openssl (= 3.1.0) compatible with your Ruby & RubyGems
        openssl requires Ruby version >= 2.6.0. The current ruby version is 2.5.0.
```
**Solution:**  
Upgrade ruby to 2.6.0

### Gem::RemoteFetcher::FetchError

Command: `gem install jekyll bundler`  
```shell
Fetching: public_suffix-5.0.1.gem (100%)
ERROR:  While executing gem ... (Gem::FilePermissionError)
    You don't have write permissions for the /var/lib/gems/2.5.0 directory.

    ERROR:  Error installing jekyll:
    The last version of public_suffix (< 6.0, >= 2.0.2) to support your Ruby & RubyGems was 4.0.7.
    Try installing it with `gem install public_suffix -v 4.0.7` and then running the current command again
    public_suffix requires Ruby version >= 2.6. The current ruby version is 2.5.0.

    ERROR:  Error installing bundler:
    The last version of bundler (>= 0) to support your Ruby & RubyGems was 2.3.26.
    Try installing it with `gem install bundler -v 2.3.26`
    bundler requires Ruby version >= 2.6.0. The current ruby version is 2.5.0.
                 
    ERROR:  While executing gem ... (Gem::RemoteFetcher::FetchError)
    bad response Forbidden 403 (https://api.rubygems.org/quick/Marshal.4.8/google-protobuf-3.22.3-x64-unknown.gemspec.rz)
```
**Solution:**  
Remove Ruby v2.5 and run through the instructions again  

### Error installing jekyll
Command: `gem install jekyll bundler`

```shell
ERROR:  Error installing jekyll:
        The last version of sass-embedded (~> 1.54) to support your Ruby & RubyGems was 1.58.3.
        Try installing it with `gem install sass-embedded -v 1.58.3` and then running the current command again
        sass-embedded requires Ruby version >= 2.7.0. The current ruby version is 2.6.6.146.
```
**Solution:**
`sudo gem update --system`  

{: .notice--primary}
<strong>References:</strong>  
[Jekyll - Static site generator](https://jekyllrb.com/)  
[Windows Subsystem for Linux (WSL)](https://learn.microsoft.com/en-us/windows/wsl)  
[github - Filesystem watchers like libinotify do not work](https://github.com/microsoft/WSL/issues/216)