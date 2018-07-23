{:title "cost comparison google cloud vs digitalocean"
:layout :post
 :tags ["old-post"]}



I've been using a VM within the Google Compute Engine cloud, for the last two months (mostly from my chromebook, occasionally from a couple of other locations).



Specifically, this is a "small" (`g1-small`) VM, though there are [many other options available](https://developers.google.com/compute/docs/machine-types).



This comes with a single CPU core and 1.7G of memory (the disk space is provisioned separately).



For perspective, the only one smaller than this is an `f1-micro` instance (also with a single core, but with 0.6G of memory).



So it makes sense to look at what this costs ...



{% img center http://farm3.staticflickr.com/2818/12124686505_6db70c0917_o_d.png %}



For comparison, [Digital Ocean](https://www.digitalocean.com/) has far less flexibility, but I just might switch to it, considering that I have absolutely no plans to ever host a user-facing service or do anything of that sort, on my VM (if I did, there really wouldn't be any point; you want those features, folks -- network protection, scalability, all that jazz).



So long as it's just little old me accessing my portable home in the cloud, the $10/mo for a 1G/30G SSD option looks just as good (or  $5/mo for 512M; also, DigitalOcean keeps running free trial offers, which are a good way to try out the service).


