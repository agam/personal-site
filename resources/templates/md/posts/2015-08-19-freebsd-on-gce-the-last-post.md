{:title "freebsd on gce the last post"
:layout :post
 :tags ["old-post"]}



After [one unsuccessful attempt](blog/2014/12/14/trying-out-freebsd-and-failing-at-it/) and one [hack-ey successful one](blog/2015/06/13/freebsd-on-google-compute-engine/), getting FreeBSD up and running on GCE is now a one-liner:



```sh

$ gcloud compute instances create pandora --image freebsd-10-2-release-amd64 --image-project=freebsd-org-cloud-dev

$ gcloud compute ssh agam@pandora

```



... and that's it. **Kudos** to whoever made this happen!
