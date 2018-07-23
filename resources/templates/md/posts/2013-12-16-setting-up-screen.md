{:title "setting up screen"
:layout :post
 :tags ["old-post"]}



This is pretty much the first thing I do. I've learned the hard way that I really really need persistent terminal state.



One of the changes I always make is to switch the default key combination away from `C-a`. Initially I did this during my "year of trying Emacs" (since it's the default "move to beginning of line" shortcut). I chose `C-z` (via `escape ^Zz`) but you might want to do this anyway if for example you feel `C-b` would be easier to type.



The other must-have is labels for the tabs. I want to know what I'm switching between (a suitable modification to `hardstatus`).



The status line looks something like this.



![screen status with 3 tabs open](https://pbs.twimg.com/media/BblUW36CQAAG_Ol.png:large)



It's pretty minimal; you can add an arbitrary amount of customization to your status line, but this is all that I need.



Finally, there are a couple of "features" that _have_ to be disabled: the screen flash on tab completion (`vbell off`), and the annoying startup message (`startup_message off`).



I'm sure you know all about switching tabs, detaching and resuming sessions and all that. The only (in hindsight obvious) rule of thumb I've learnt is to _always_ launch a session with a name. Because you might just want to start two sessions. And then when it comes time to resume them, you won't know which is which. So make `screen -S <sessionname>` your friend.



Anyway I have a github repo for my "dotfiles" and you can see my basic screenrc [here](https://github.com/agam/dotfiles/blob/master/.screenrc)
