{:title "urbit hoon"
:layout :post
 :tags ["old-post"]}







First off, just in case you closed your shell, terminal, screen session, whatever: you can easily resume your Arvo session



```sh

agam@myvm:~/urbit$ bin/vere frogstar

vere: urbit home is /home/agam/urbit/urb

loom: loaded 11MB

init: gc: 2.361.752 bytes live

time: ~2014.1.7..09.04.14..a902

ames: on localhost, UDP 57882.

http: live on 8080

rest: checkpoint to event 7.319

rest: old 0v1i.p6o91, new 0v1k.b3ul0

loaded passcode from /home/agam/.urbit/~fiddyn-lavpun.txt

rest: replaying through event 7323

---------------- playback starting----------------

.....

---------------- playback complete----------------

save: gc: 28 bytes shed; 2.361.836 bytes live



~wicmet-balful-tipdet-tardul--nisred-divpen-panref-sicbet/try=> 

```



(hmm ... "*playback complete*" ?? Yeah apparently everything you ever do is known forever, but that makes my head hurt right now so I won't try too hard to understand it just yet)



Alright, continuing on ...



## Fun at the REPL



If you remember from [earlier on](http://agam.github.io/blog/2014/01/06/urbit/), we were dropped into a REPL.



Being a REPL means we can do stuff like



```sh

~wicmet-balful-tipdet-tardul--nisred-divpen-panref-sicbet/try=> (add 4 5)

9

```



and



```sh

~wicmet-balful-tipdet-tardul--nisred-divpen-panref-sicbet/try=> =a 17

~wicmet-balful-tipdet-tardul--nisred-divpen-panref-sicbet/try=> (add 14 a)

31

```



and also



```sh

~wicmet-balful-tipdet-tardul--nisred-divpen-panref-sicbet/try=> (mul 6 7)

42

```



or



```sh

~wicmet-balful-tipdet-tardul--nisred-divpen-panref-sicbet/try=> (mul 5 (add 4 7))

55

```



If you look at some of the `hoon` files that were downloaded at the initial setup, you might intuit that they are executable-ish.



```sh

+ /~wicmet-balful-tipdet-tardul--nisred-divpen-panref-sicbet/try/1/bin/reverse/hoon

...

+ /~wicmet-balful-tipdet-tardul--nisred-divpen-panref-sicbet/try/1/bin/fib/hoon

...

```

and more.



These can be run ("evaluated" !!) at the command line. E.g. for `fib/hoon`:



```sh

~wicmet-balful-tipdet-tardul--nisred-divpen-panref-sicbet/try=> :fib 0

1

~wicmet-balful-tipdet-tardul--nisred-divpen-panref-sicbet/try=> :fib 1

1

~wicmet-balful-tipdet-tardul--nisred-divpen-panref-sicbet/try=> :fib 2

2

~wicmet-balful-tipdet-tardul--nisred-divpen-panref-sicbet/try=> :fib 3

3

~wicmet-balful-tipdet-tardul--nisred-divpen-panref-sicbet/try=> :fib 4

5

```



Or for `zero/hoon`:



```sh

~wicmet-balful-tipdet-tardul--nisred-divpen-panref-sicbet/try=> :zero

0                             ::  0, ~zod

0                             ::  1, ~nec

0                             ::  2, ~bud

0                             ::  3, ~wes

0                             ::  4, ~sev

0                             ::  5, ~per

0                             ::  6, ~sut

0                             ::  7, ~let

0                             ::  8, ~ful

...

0                             ::  252, ~pec

0                             ::  253, ~nel

0                             ::  254, ~nev

0                             ::  255, ~fes

```



which is just 256 zeros ... but you get the idea.



## Hoon files



(More on what `hoon` really is and how to parse it later (it has to be later because I don't know WTF it is right now))



As another example, let's examine `cat/hoon` (which fulfills a purpose similar to the Unix `cat` you're familiar with)



```sh

~wicmet-balful-tipdet-tardul--nisred-divpen-panref-sicbet/try=> :cat /=main=/toy/cat/hoon

~ /cx/~wicmet-balful-tipdet-tardul--nisred-divpen-panref-sicbet/main/~2014.1.7..09.24.24..4de6/toy/cat/hoon

```



The "real" file is at a location *and* a date! Yes, seriously. Running it again ...



```sh

~wicmet-balful-tipdet-tardul--nisred-divpen-panref-sicbet/try=> :cat /=main=/toy/cat/hoon

~ /cx/~wicmet-balful-tipdet-tardul--nisred-divpen-panref-sicbet/main/~2014.1.7..09.25.22..9bed/toy/cat/hoon

```



... will give a different path.



So far so good. What does the file actually contain ?



```sh

~wicmet-balful-tipdet-tardul--nisred-divpen-panref-sicbet/try=> :cat /=main=/bin/cat/hoon

'!:'

'::  /=main=/toy/cat/hoon'

'::'

'|=  *'

'|=  ape=(list path)'

':_  ~'

':_  ~'

'^-  gift'

':+  %mu  [%atom %t]'

'=-  |-  ^-  (list ,@t)'

'    ?~(foz ~ (weld i.foz $(foz t.foz)))'

'^=  foz'

'=|  foz=(list (list ,@t))'

'|-  ^+  foz'

'?~  ape  ~'

'[(lore ((hard ,@) .^(%cx i.ape))) $(ape t.ape)]'

```



"Yikes" is an appropriate response at this point.



BTW in case you were wondering: yes, right now Urbit *is* hosted on top of Unix (**Arvo** is written in **Hoon**, which is written in **Nock**, which is written in **C**), so you should see the exact same contents if you run



```sh

$ cat wicmet-balful-tipdet-tardul--nisred-divpen-panref-sicbet/main/bin/cat.hoon

```



## Setting up a destroyer



So I asked the nice folks at [urbit](mailto:urbit@urbit.org) nicely and they sent me a (nice?) pair of destroyers (which if nothing else, should shorten the prompt by 75% or so).



You'll receive a *ship* and its *ticket*, and then run



```sh

~wicmet-balful-tipdet-tardul--nisred-divpen-panref-sicbet/try=> :begin ~todbyt-nathes



Launching ~todbyt-nathes, one of 4.278.124.800 Urbit destroyers...



            If I did not build for myself

            for whom should I build?



                  -- Bunting, _Chomei at Toyama_



Let's configure your identity.  Warning - it's nontrivial to

change this data once you've sent it to the server.  If you enter

something wrong, hit ^D to cancel, then run the program again.



Your ticket: <Enter your ticket here>



Enter a passphrase or other unusual text.  You (or your enemies)

can regenerate your ship from this entropy.



Entropy: **********

Entropy check: ~nildyl-hocmun

```



Customization follows. This would normally be boring boilerplate, but I've chosen to reproduce it here since some if it is entertaining.



```sh

What language would your ship rather speak?  Enter it as a

two-letter (ISO 639-1) code, like "en" for English.  Whatever

language you pick, it'll all be English now, but in future

we'll be more sensitive to your needs.



Language: en



Okay, we'll be speaking English.

```

```sh

Please select one of the pre-chosen forms:



  %lady   female-identified individual

  %lord   male-identified individual

  %punk   opaque handle

  %anon   totally anonymous



Form: %lord



You've selected a personal identity.  For a %lord or a %lady,

please use your real name only.  If you'd prefer a fictitious

handle, you can hit ^D and start over as a %punk.  Or you can

use %anon and go by your ship name alone.

```

```sh

As a real person, we're going to ask you for a little bit of

personal information - not enough to compromise your privacy,

just enough to make everyone on Urbit feel like a neighbor.



What we're going to ask you for is information that's (a)

public (would be obvious, or at least easy to guess, for

someone who met you in person); (b) durable (doesn't change

often); and (c) relevant (helps you connect with friends).



For example, we'll ask for the year (not day) you were born,

because your age is easy to guess in person, doesn't change,

and has a big effect on how people perceive you.



```

```sh

Year you were born: <Enter your year of birth here>



Where are you?  This is totally optional, but we'd like to

know your vague general location.  You can enter nothing at

all, just a country code, or country and postal code.



Location: us/<Enter your zipcode here>



Please tell us your name - and please make it a real name.

Pseudonyms are totally cool, but please use %punk.  Middle

name and nickname are optional.  Ünicode is encouraged.



First name: <Enter your name here>

Middle name (or blank):

Nickname/handle (or blank): agam

Last name: <Enter your name here>

```



```sh

One basic human instinct is the urge to form political tribes.

In a network that fights this need, these tribes form anyway and

they form badly.  Urbit designs them into the infrastructure.

Inspired by the Qing Dynasty, you'll align yourself with one of

five colored "banners" - %red, %blue, %orange, %black or %white.



Political discourse across tribal boundaries is almost always an

antisocial act - less communication, than symbolic violence.  In

Urbit, messages marked political stay within your own banner;

your friends in other banners simply won't see them.  Between

banners, politics doesn't erode apolitical relationships; inside

each banner, political discourse is harmonious and productive.



Pick a banner by the adjective you feel best describes you:



%red     far left: radical, anarchist

%blue    center-left: moderate, liberal

%orange  center-right: conservative, libertarian

%black   far right: traditionalist, reactionary



Or if digital agitation isn't your cup of tea, choose %white, and

your Urbit experience will remain politics-free.



Banner: %white

```



At this point, it got stuck ...



```sh

generating 2048-bit RSA key...

ames: czar zod.urbit.org: ip .198.199.112.32

; ~doznec ~doznec not responding still trying

waiting on ~doznec/ta

```



... so I hit `Ctrl-D` and ran it again.



Still nothing. I guess I'll continue the next post once I get this working (!)
