20240623, 2:42 pm
Previous personal page, last updated July 16, 2017: https://github.com/jwt625/jwt625.github.io
Reading https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/about-github-pages-and-jekyll

Installing jekyll following chatGPT
```
brew install rbenv
rbenv init

rbenv install 3.1.0
rbenv global 3.1.0
ruby -v
gem install bundler
gem install jekyll
```

2024-06-23T14:56:42-07:00 took quite a while to install `3.1.0`
Got error installing bundler:
```
Fetching bundler-2.5.14.gem

ERROR:  While executing gem ... (Gem::FilePermissionError)

    You don't have write permissions for the /Library/Ruby/Gems/2.6.0 directory.
```
sudo it.
2024-06-23T15:03:39-07:00 New error:
```
ERROR:  Error installing bundler:

The last version of bundler (>= 0) to support your Ruby & RubyGems was 2.4.22. Try installing it with `gem install bundler -v 2.4.22`

bundler requires Ruby version >= 3.0.0. The current ruby version is 2.6.10.210.
```
Installing that version. Now installing `jekyll`.
Got same version error.
Fixed ruby version by adding the following to `.zshrc` and `.bash_profile`
```
# Add rbenv to PATH for access to the rbenv command-line utility
export PATH="$HOME/.rbenv/bin:$PATH"

# Load rbenv automatically by appending the following to ~/.bash_profile or ~/.zshrc
eval "$(rbenv init -)"

```
Reinstalling bundler and jekyll. Done.
2024-06-23T15:14:39-07:00 Server running.


