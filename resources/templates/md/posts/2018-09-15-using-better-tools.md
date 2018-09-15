{:title "Using better tools"
 :layout :post
 :tags ["tools"]}


If there’s one mantra I like to leave people with, it’s _Use Good Tools!_ … or, as I like to phrase it differently sometimes, _Use the right tool for the job!_

Stop clinging to old tools. It’s hard, since you have all that muscle memory, and you don’t want to lose what is sometimes hard-won knowledge, but … you still _have_ to let it go. Sometimes, you have no idea how much _better_ it could be!

Here is a list I came up with; I split it up into tools I’ve been using for _years_ now, and ones that are more recent ones, since while the latter are still useful, I can’t vouch for them with the same “_D’oh_, you should _obviously_ be using this!” level of enthusiasm.

Long-time use (_highly recommend_):
- **`zsh`** or **`fish`**: Just pick one, doesn’t matter. If you can’t pick: _just go with [Oh-my-zsh](https://www.github.com/robbyrussell/oh-my-zsh)\_
- **`rtags`**: if you use C++ inside Emacs, make your life better
- **`tmux`**: are you _still_ opening multiple terminal windows? Don’t.
- **`mosh`**: are you _still_ re-ssh-ing every time you open your laptop or VPN in? Don’t.
- **`fzf`**: anytime you need to pick options at the command-line; at the very least, the _only_ way you should be referencing command history.
- **`helm`**: If you use Emacs, are you still typing more than two or three characters per-operation? Don’t.
- **`magit`**: If you use Emacs, this should be the _only_ way you interact with `git`.
- **`ag`**: Stop using `grep`! (or `ripgrep` instead, I hear it’s as good)
- **`jq`**: If you work with json output, _duh_.
- **`htop`**: Stop using `top`.

Recent discoveries (_cautiously recommend_):
- **`fd`**: dead-simple replacement for `find` 
- **`exa`**: prettier-looking replacement for `ls`
- **`peco`**: similar to `fzf`? 
- **`tig`**: for when you want to use `git` outside Emacs
- **`lnav`**: getting around log files
- **`bat`**: context-aware replacement for `cat` and `less`
- **`pydf`**: dead-simple replacement for `df`


