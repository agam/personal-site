{:title "augmenting my chromebook with a vm"
:layout :post
 :tags ["old-post"]}



I had tried this out about a year ago, creating a small VM instance on Google Compute Engine. But a maintenance window occurred, my VM died, and that was that. I cleaned up and walked away.



Comparison

----------



Sometime back, I heard about [Digital Ocean](https://www.digitalocean.com/) from a friend. I had just switched to a chromebook and was missing the shell of my previous Linux laptop. So I once again looked into the current cloud offerings.



There is an excellent comparison at [cloud price grid](http://cloudpricegrid.com/) so I won't waste time repeating that.



DigitalOcean seems the cheapest for a couple of options



* _1core/512Mb/20G --> $5/mo_

* _1core/1G/30G  --> $10/mo_



(See [detailed pricing](https://www.digitalocean.com/pricing/))



However, this is without any firewall or networking capability, and the datacenter seems to be near New York, so that's something to keep in mind.



Meanwhile at Google Compute Engine, the right comparison would be with a combination of



* _Persistent disk --> 20G for $0.8/mo_

* _f1-micro [1core/600Mb] --> $0.019/hr_

[which by the way at max utilization turns out to be $13.78/mo]



(See [detailed pricing](https://cloud.google.com/products/compute-engine/#pricing))



A similar comparison for Amazon (I could be off in apples-to-apples here (what I think are apples may in fact be oranges))



_t1.micro [1core/630Mb] --> $0.025/hr_



So the cheapest Amazon has to offer seems more expensive than the cheapest Google has to offer (both of these instances will be pretty useless for hosting something substantial, but we're assuming the idea here is to have a small "developer space").



But keep in mind Amazon also has a [free tier](http://aws.amazon.com/free/) so if you just want to try it out for a year, the micro instance is actually FREE (!)



Getting (re-)started with Google Compute Engine

-----------------------------------------------



I elected to stay with Google. The new [cloud console](https://cloud.google.com/console#/project) is much better than it was before, with a nice grouping by project and sub-links to Appengine, CloudSql, CloudStorage, ComputeEngine, etc.



Within the Cloud Engine dashboard, it is possible (and in fact, easy) to create a new VM, set up its associated disk and static IPs, OS, etc from the UI.



SSHing

------



One thing to point out is that the first time I set it up I used the `gcutil` tool to ssh into the instance; this creates local ssh keys and uploads the public key to the instance (it can be viewed in the `Metadata` part of the console). So I had to do this from a non-chromebook "real" computer (though there may be a workaround I'm unaware of).



For subsequent SSHs, the recommended way is to use a long SSH command



```sh

ssh -o UserKnownHostsFile=/dev/null -o CheckHostIP=no -o  StrictHostKeyChecking=no \

 -i <key-file> -o LogLevel=QUIET -A  -p 22 <username>@<ipaddress>

```



and this worked when I tried it from the computer I signed up on.



Before trying to SSH from a chromebook, you have to copy ssh keys locally (how? I Dropbox'd them and then deleted them from Dropbox, which is admittedly less secure ... maybe I should have written it down and recreated manually?!).



Anyway, so once you have copied the keys, you can use the [Secure Shell](https://chrome.google.com/webstore/detail/secure-shell/pnhechapfaindjhompbnflcldabbghjo?hl=en) app.



You have to click on `import...` to select the public/private key pair. At this point, when I used the long SSH command mentioned above, it didn't work. Eventually it only worked when I tried with a blank command (so the only arguments were username, host IP and the key file).



Chromebook + VM

---------------



Having tried this out for a while, I can't imagine living without it. Obviously it depends on internet access, but that's a common caveat these days.



But I strongly encourage it, since it's a perfect complement to the otherwise "browser-only" world.


