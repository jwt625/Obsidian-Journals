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

Ok time to go back to reading jekyll for blogging.

Asked chatGPT to recommend me a few templates. Decide to try out 
- https://github.com/mmistakes/minimal-mistakes
Configuration:
- https://mmistakes.github.io/minimal-mistakes/docs/configuration/

Tried directly running `jekyll serve` and did not work:
```
Ws-MacBook-Pro:ofs w$ jekyll serve

/Users/w/.rbenv/versions/3.1.0/lib/ruby/gems/3.1.0/gems/bundler-2.5.14/lib/bundler/resolver.rb:336:in `raise_not_found!': **Could not find gem 'github-pages' in locally installed gems. (****Bundler::GemNotFound****)**


```

Go back to the readme and following the installation -> remote-theme method:
- https://github.com/mmistakes/minimal-mistakes?tab=readme-ov-file#remote-theme-method
Running `bundle`.
- seeing it fetching a whole lot of different shit. I do not like all these dependencies.

2024-06-23T15:36:49-07:00 Done following the readme. Got redirected to:
- https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/

2024-06-23T22:04:23-07:00 Setting up jekyll on windows desktop
- installing Ruby with devkit
- `gem install jekyll bundler`
	- it runs for quite a while


Running the website: `bundle exec jekyll serve`
- have to run `bundle install` first on the windows machine.
- `wdm` install throws error. ChatGPT suggests installing it manually.
```

Using rubygems directory: C:/Users/Wentao/.local/share/gem/ruby/3.3.0
Temporarily enhancing PATH for MSYS/MINGW...
Building native extensions. This could take a while...
ERROR:  Error installing wdm:
        ERROR: Failed to build gem native extension.

    current directory: C:/Users/Wentao/.local/share/gem/ruby/3.3.0/gems/wdm-0.1.1/ext/wdm
C:/Ruby33-x64/bin/ruby.exe extconf.rb
checking for -lkernel32... yes
checking for windows.h... yes
checking for ruby.h... yes
checking for HAVE_RUBY_ENCODING_H... yes
checking for rb_thread_call_without_gvl()... yes
creating Makefile

current directory: C:/Users/Wentao/.local/share/gem/ruby/3.3.0/gems/wdm-0.1.1/ext/wdm
make DESTDIR\= sitearchdir\=./.gem.20240623-11948-m2cf5j sitelibdir\=./.gem.20240623-11948-m2cf5j clean

current directory: C:/Users/Wentao/.local/share/gem/ruby/3.3.0/gems/wdm-0.1.1/ext/wdm
make DESTDIR\= sitearchdir\=./.gem.20240623-11948-m2cf5j sitelibdir\=./.gem.20240623-11948-m2cf5j
generating wdm_ext-x64-mingw-ucrt.def
compiling entry.c
compiling memory.c
compiling monitor.c
compiling queue.c
compiling rb_change.c
rb_change.c: In function 'extract_absolute_path_from_notification':
rb_change.c:139:5: warning: 'RB_OBJ_TAINT' is deprecated: taintedness turned out to be a wrong idea. [-Wdeprecated-declarations]
  139 |     OBJ_TAINT(path);
      |     ^~~~~~~~~
In file included from C:/Ruby33-x64/include/ruby-3.3.0/ruby/internal/core/rstring.h:30,
                 from C:/Ruby33-x64/include/ruby-3.3.0/ruby/internal/arithmetic/char.h:29,
                 from C:/Ruby33-x64/include/ruby-3.3.0/ruby/internal/arithmetic.h:24,
                 from C:/Ruby33-x64/include/ruby-3.3.0/ruby/ruby.h:28,
                 from C:/Ruby33-x64/include/ruby-3.3.0/ruby.h:38,
                 from wdm.h:22,
                 from rb_change.c:4:
C:/Ruby33-x64/include/ruby-3.3.0/ruby/internal/fl_type.h:824:1: note: declared here
  824 | RB_OBJ_TAINT(VALUE obj)
      | ^~~~~~~~~~~~
compiling rb_monitor.c
rb_monitor.c: In function 'rb_monitor_run_bang':
rb_monitor.c:509:29: error: implicit declaration of function 'rb_thread_call_without_gvl' [-Wimplicit-function-declaration]
  509 |         waiting_succeeded = rb_thread_call_without_gvl(wait_for_changes, monitor->process_event, stop_monitoring, monitor);
      |                             ^~~~~~~~~~~~~~~~~~~~~~~~~~
make: *** [Makefile:248: rb_monitor.o] Error 1

make failed, exit code 2

Gem files will remain installed in C:/Users/Wentao/.local/share/gem/ruby/3.3.0/gems/wdm-0.1.1 for inspection.
Results logged to C:/Users/Wentao/.local/share/gem/ruby/3.3.0/extensions/x64-mingw-ucrt/3.3.0/wdm-0.1.1/gem_make.out
```


Ugh seems like I need to install an older version of Ruby...
- going to switch to `3.0.7`
- `bundle install` sees to run through

Now `bundle exec jekyll serve`
Yes it runs and serves.

# Examples (other people's blogs) to learn from

https://www.willwhang.dev/

https://lilianweng.github.io/

https://www.jacobwright.xyz/

https://lumingtang.info/

https://cld.sh/index.php

https://bithole.dev/

https://lawtonskaling.sites.stanford.edu/