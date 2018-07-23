{:title "freebsd on google compute engine"
:layout :post
 :tags ["old-post"]}



I had tried running FreeBSD on Google Compute Engine previously, but [given up](blog/2014/12/14/trying-out-freebsd-and-failing-at-it/). Last week I was suddenly reminded of this, and after Googling around a bit, came across [this script](https://github.com/swills/FreeBSD-gcloud) by Steve Wills _(Thanks, Steve!)_ that automated the task of building a "GCE-ready" image on an existing FreeBSD system.



Hooray! No, wait, I _don't_ have an existing FreeBSD system; that's sort of what this is all about. This minor despondency was short-lived, since I had temporarily forgotten how we now live with computers within computers within computers (give or take a layer). So I installed the [VMWare Workstation](http://www.vmware.com/products/workstation), downloaded [the iso](ftp://ftp.freebsd.org/pub/FreeBSD/releases/amd64/amd64/ISO-IMAGES/10.1/) for `10.1-RELEASE`, and booted it up.



All you need is the Google Cloud SDK, which is available under the ports tree, so something like the following should suffice:



```sh

$ cd /usr/ports/net/google-cloud-sdk

$ make install

```



Then grab the script and run it.



```sh

$ curl https://raw.githubusercontent.com/swills/FreeBSD-gcloud/master/build_google_image.sh > /tmp/swill-script.sh

$ chmod a+x /tmp/swill-script.sh

$ /tmp/swill-script.sh

components

base.txz                                      100% of   63 MB 7041 kBps 00m09s

kernel.txz                                    100% of   84 MB 3187 kBps 00m27s

Creating image

Creating filesystem

Installing base into image

Installing kernel into image

Setting up image

Finishing image

Creating partitioned file

Creating image tar

Now run:



gcloud auth login

gsutil cp FreeBSD-10.1-RELEASE-amd64-20150612191810.tar.gz gs://swills-test-bucket

gcutil addimage freebsd-101-release-amd64-20150612191810 gs://swills-test-bucket/FreeBSD-10.1-RELEASE-amd64-20150612191810.tar.gz

```



As you might have noticed, the script ends with a helpful suggestion for the subsequent commands to run. Caveats:



- If you're doing all this within a VM like I was, you'll want to run `gcloud auth login --no-launch-browser`

- You might need to run `gcloud config set project <project ID>` (that's project _ID_, not project _name_)

- (obviously) replace `swills-test-bucket` with your bucket (you can create one at the [cloud console](https://console.developers.google.com) (Storage -> Cloud Storage -> Storage Browser)



After this, a few clicks should get you a new instance (remember to select the custom image uploaded earlier), and SSHing into it should show you `/etc/motd` (which is still a plain old file, and not, you know, [this abomination](https://web.archive.org/web/20120107210839/http://deadmemes.net/2010/10/19/fear-and-loathing-in-debianubuntu-or-who-needs-etcmotd)).



**Enjoy!**
