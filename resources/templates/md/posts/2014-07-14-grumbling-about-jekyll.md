{:title "grumbling about jekyll"
:layout :post
 :tags ["old-post"]}



Yeah, not happy about how these dependencies keep breaking. It's a reminder about how fragile these scripting ecosystems are.



Seeing ``Gem::LoadError: You have already activated rake 10.2.2, but your Gemfile requires rake 0.9.6.``` once is enough.



I tried a bunch of stuff and none of it worked. Eventually I gave up, deleted `Gemfile.lock`, ran `bundle install`, and just switched my workflow over to using `bundle exec rake ...` instead of just `rake ...`. Which grates on me now, but whatever.



Oh wait, no that changed *nothing*. Bundle is still using `rake 0.9.6`. Do I need to muck around in the Gemfile now? Why can't it just, you know use this "activated version" instead?



Nooo ... I have an old version of ruby. I know, I'll use `rvm`, which I used when I got started with all this. Except ...



```

RVM is not a function, selecting rubies with 'rvm use ...' will not work.



You need to change your terminal emulator preferences to allow login shell.

Sometimes it is required to use `/bin/bash --login` as the command.

Please visit https://rvm.io/integration/gnome-terminal/ for a example.

```



There _must_ be a way out. Doesn't Ubuntu update ruby? Let's try `sudo apt-get upgrade ruby`. No, it's already the "latest version".



What? Ah yes, the breakdown in sensible versioning: different versions are just different names here, let's try `sudo apt-get install ruby1.9` instead.



```

The following packages have unmet dependencies:

 libodbc-ruby1.9.1 : Depends: ruby-odbc but it is not going to be installed

 libodbc-ruby1.9.1-dbg : Depends: ruby-odbc-dbg but it is not going to be installed

E: Unable to correct problems, you have held broken packages.

```



I have "held broken packages"! Indeed! Regardless, something seems to have happened, so `sudo ln -sf /usr/bin/ruby1.9.1 /usr/bin/ruby` might do something ?



Alriiite! Let's try to use `rake` again:



```

$ bundle exec rake generate

Could not find rake-0.9.6 in any of the sources

```



Hmm, perhaps I need to rerun `bundle update` ?



```

Installing RedCloth (4.2.9) with native extensions /usr/lib/ruby/1.9.1/rubygems/installer.rb:552:in `rescue in block in build_extensions': ERROR: Failed to build gem native extension. (Gem::Installer::ExtensionBuildError)



        /usr/bin/ruby1.9.1 extconf.rb 

/usr/lib/ruby/1.9.1/rubygems/custom_require.rb:36:in `require': cannot load such file -- mkmf (LoadError)

						     from /usr/lib/ruby/1.9.1/rubygems/custom_require.rb:36:in `require'

						     from extconf.rb:1:in `<main>'



```



Clearly, I am the stupid n00b here. I've managed to end up worse off than where I started. Help!



Time to try something different. Overwrite the Gemfile with [the octopress master copy](https://github.com/imathis/octopress/blob/master/Gemfile) and run `bundle install` again.



BTW, did I mention how long this step takes? No? Too bad. You'll just have to find out for yourself.



<lolrus>Nooo! I still be getting error!</lolrus>



What does the great and benevolent [Stackoverflow suggest](http://stackoverflow.com/questions/12149503/snorby-setup-redcloth-error)? `sudo apt-get install ruby1.9.1-dev`



At this point the magnitude of my true dependencies was revealed to me:



```

Fetching source index for https://rubygems.org/

Using rake (10.3.2) 

Installing RedCloth (4.2.9) with native extensions 

Installing blankslate (2.1.2.4) 

Installing timers (1.1.0) 

Installing celluloid (0.15.2) 

Installing chunky_png (1.3.1) 

Installing fast-stemmer (1.0.2) with native extensions 

Installing classifier (1.3.4) 

Installing coffee-script-source (1.7.1) 

Installing execjs (2.2.1) 

Installing coffee-script (2.3.0) 

Installing colorator (0.1) 

Installing fssm (0.2.10) 

Installing sass (3.2.19) 

Installing compass (0.12.6) 

Installing ffi (1.9.3) with native extensions 

Installing tilt (1.4.1) 

Installing haml (4.0.5) 

Installing jekyll-coffeescript (1.0.0) 

Installing jekyll-gist (1.1.0) 

Installing jekyll-paginate (1.0.0) 

Installing jekyll-sass-converter (1.0.0) 

Installing rb-fsevent (0.9.4) 

Installing rb-inotify (0.9.5) 

Installing listen (2.7.9) 

Installing jekyll-watch (1.0.0) 

Installing kramdown (1.4.0) 

Installing liquid (2.6.1) 

Installing mercenary (0.3.4) 

Installing posix-spawn (0.3.8) with native extensions 

Installing yajl-ruby (1.1.0) with native extensions 

Installing pygments.rb (0.6.0) 

Installing redcarpet (3.1.2) with native extensions 

Installing safe_yaml (1.0.3) 

Installing parslet (1.5.0) 

Installing toml (0.1.1) 

Installing jekyll (2.1.1) 

Installing jekyll-date-format (1.0.0) 

Installing jekyll-page-hooks (1.3.0) 

Installing jekyll-sitemap (0.5.0) 

Installing rack (1.5.2) 

Installing rack-protection (1.5.3) 

Installing rdiscount (2.0.7.3) with native extensions 

Installing rubypants (0.2.0) 

Installing sass-globbing (1.0.0) 

Installing sinatra (1.4.5) 

Installing stringex (1.4.0) 

Using bundler (1.0.15) 

Your bundle is complete! It was installed into ./liquid

```



After that cheeful status update, I thought I'd start by trying out the old workflow again:



```

$ rake generate

Could not find rake-10.3.2 in any of the sources

Run `bundle install` to install missing gems.

```



Too optimistic too soon, eh?



```

$ bundle exec rake generate

## Generating Site with Jekyll

overwrite source/stylesheets/screen.css 

/usr/local/google/home/agam/Documents/agam.github.io/liquid/ruby/1.9.1/gems/execjs-2.2.1/lib/execjs/runtimes.rb:51:in `autodetect': Could not find a JavaScript runtime. See https://github.com/sstephenson/execjs for a list of available runtimes. (ExecJS::RuntimeUnavailable)

														      from /usr/local/google/home/agam/Documents/agam.github.io/liquid/ruby/1.9.1/gems/execjs-2.2.1/lib/execjs.rb:5:in `<module:ExecJS>'

														      from /usr/local/google/home/agam/Documents/agam.github.io/liquid/ruby/1.9.1/gems/execjs-2.2.1/lib/execjs.rb:4:in `<top (required)>'

														      from /usr/local/google/home/agam/Documents/agam.github.io/liquid/ruby/1.9.1/gems/coffee-script-2.3.0/lib/coffee_script.rb:1:in `require'

														      from /usr/local/google/home/agam/Documents/agam.github.io/liquid/ruby/1.9.1/gems/coffee-script-2.3.0/lib/coffee_script.rb:1:in `<top (required)>'

														      from /usr/local/google/home/agam/Documents/agam.github.io/liquid/ruby/1.9.1/gems/coffee-script-2.3.0/lib/coffee-script.rb:1:in `require'

														      from /usr/local/google/home/agam/Documents/agam.github.io/liquid/ruby/1.9.1/gems/coffee-script-2.3.0/lib/coffee-script.rb:1:in `<top (required)>'

														      from /usr/local/google/home/agam/Documents/agam.github.io/liquid/ruby/1.9.1/gems/jekyll-coffeescript-1.0.0/lib/jekyll-coffeescript.rb:2:in `require'

														      from /usr/local/google/home/agam/Documents/agam.github.io/liquid/ruby/1.9.1/gems/jekyll-coffeescript-1.0.0/lib/jekyll-coffeescript.rb:2:in `<top (required)>'

														      from /usr/local/google/home/agam/Documents/agam.github.io/liquid/ruby/1.9.1/gems/jekyll-2.1.1/lib/jekyll.rb:75:in `require'

														      from /usr/local/google/home/agam/Documents/agam.github.io/liquid/ruby/1.9.1/gems/jekyll-2.1.1/lib/jekyll.rb:75:in `<top (required)>'

														      from /usr/local/google/home/agam/Documents/agam.github.io/liquid/ruby/1.9.1/gems/jekyll-2.1.1/bin/jekyll:6:in `require'

														      from /usr/local/google/home/agam/Documents/agam.github.io/liquid/ruby/1.9.1/gems/jekyll-2.1.1/bin/jekyll:6:in `<top (required)>'

														      from /usr/local/google/home/agam/Documents/agam.github.io/liquid/ruby/1.9.1/bin/jekyll:19:in `load'

														      from /usr/local/google/home/agam/Documents/agam.github.io/liquid/ruby/1.9.1/bin/jekyll:19:in `<main>'

```



Obviously, there's one dependency missing from the list above. What, you say, you _didn't_ know you need to install `Node.js` too?



`sudo apt-get install nodejs`



**Are we there yet?** The suspense is killing me. Nope, running `bundle exec rake generate` just (very helpfully!) shows me



```

$ bundle exec rake generate

## Generating Site with Jekyll

unchanged sass/screen.scss

jekyll 2.1.1 -- Jekyll is a blog-aware, static site generator in Ruby



Usage:



  jekyll <subcommand> [options]



Options:

        -s, --source [DIR]  Source directory (defaults to ./)

        -d, --destination [DIR]  Destination directory (defaults to ./_site)

            --safe         Safe mode (defaults to false)

        -p, --plugins PLUGINS_DIR1[,PLUGINS_DIR2[,...]]  Plugins directory (defaults to ./_plugins)

            --layouts DIR  Layouts directory (defaults to ./_layouts)

        -h, --help         Show this message

        -v, --version      Print the name and version

        -t, --trace        Show the full backtrace when an error occurs



Subcommands:

  build                 Build your site

  docs                  Launch local server with docs for Jekyll v2.1.1

  new                   Creates a new Jekyll site scaffold in PATH

  serve, server         Serve your site locally

  doctor, hyde          Search site and print specific deprecation warnings

```



Yes, I knew Jekyll was under that somewhere. That's sort of why I started using all this in the first place. Thanks for reminding me though. When I started writing this post I thought it would be a couple of lines, a quick edit-save-post, all done in five minutes. Cue evil laughter.



_What, give up now? In our moment of triumph?_ I overwrote the Gemfile earlier; what about doing the same for the Rakefile?



```

$ bundle exec rake generate

## Generating Site with Jekyll

unchanged sass/screen.scss

Configuration file: /usr/local/google/home/agam/Documents/agam.github.io/_config.yml

       Deprecation: The 'pygments' configuration option has been renamed to 'highlighter'. Please update your config file accordingly. The allowed values are 'rouge', 'pygments' or null.

            Source: source

       Destination: public

      Generating... 

jekyll 2.1.1 | Error:  undefined method `published' for <Post: /blog/2013/06/26/data-structures-haskell-vs-cpp>:Jekyll::Post

```



Now I've _retroactively_ broken stuff.



Finally, a light dawns: I should have done the following right at the beginning: `git pull origin master`. And of course, doing this now gives merge conflicts galore.



... many tens of minutes pass ...



I realized there is no alternative to delve into the source here. `lib/jekyll/post.rb' has a `published?` method, perhaps this is being called without the trailing `?` somewhere? (BTW this is EXACTLY why I **hate** dynamically-typed, uncompiled languages!).



`plugins/preview_unpublished.rb` shows a match ... so I deleted it.



Now we have a new problem ...



```

jekyll 2.1.1 | Error:  undefined method `subclasses' for Jekyll::PostFilter:Class

```



... but at least the old one is gone! We're getting closer!



Sure enough, it's another plugin: `plugins/post_filters.rb`. At this point I realize there may be many more, so I see the list of plugins [that should exist by default](https://github.com/imathis/octopress/tree/master/plugins) and get rid of any others. Turns out, it's just one more: `plugins/sitemap_generator.rb`.



No luck yet.



```

Liquid Exception: undefined method `deep_merge' for #<Hash:0x0000000277cb38> in sitemap.xml

```



Now I'm getting impatient. I just want to post a "Goodbye" post. One last post. I'll render it myself if I have to.



Ok, it's the plugins again. I got rid of `plugins/date.rb` and updated `plugins/octopress_filters.rb`.



IT WORKS! Oh god it works!



Ok, my custom theme is gone and all, but that's ok, I won't complain. _Update_: Needless to say, I got it back (after all this, surely ...)



All's well that ends well, you say? _WTF!_ Nothing new here ... the whole point of relying on stuff like this is after all to engage in this periodic ritual sacrifice of time. Which is great when you're _12_, not so much now.



I could just leave things as they are, or ... the other option is to switch away to Wordpress or Ghost or Tumblr or whatever else (thought I don't want to migrate my posts _again_, so if I do something like that I'll just leave all this stuff here, with a pointer to it ...).



_**Update**_: I realized I had once started [a Tumblr blog](http://agamposts.tumblr.com) (or is that a _tumblog_ ?), so might as well repurpose it for something other than just snippets. If you're one of the two people who read this blog (aha! I had Google Analytics enabled!), you can "add" me, or "follow" me there. Or whatever the cool kids are calling it these days.
