{:title "google cloud sharing projects"
:layout :post
 :tags ["old-post"]}



In the past (and yes, for some things, in the present too[^1]) a few Google products have had UI with ambiguous workflows, or where it wasn't clear if or how to accomplish something.



So check this out; I found it _magical_[^2], you can easily try this with multiple Google accounts. In the cloud console[^3], click on your project, then "Permissions" in the top left, and "Add Member".



Enter the account, and then select the "sharing level" of the new account (with the _same_ semantics as, say, Google Docs -- you either _own_, _edit_, or _view_) and confirm.



Log in from the other account, go to the project, view a VM, click "SSH" and _Boom_, just like that, **everything works**!



This gist[^4] shows all the steps that were needed!



```sh

# Download the command-line utilities

$ curl https://sdk.cloud.google.com | bash

 

## Authenticate

$ gcloud auth login

 

## Sanity check

$ gcloud info

 

## One-time configuration to avoid specifying command-line flags

$ gcloud config set project "<project_id>"

$ gcloud config set compute/zone us-central1-a

 

## Login!

$ gcloud compute ssh agam@myvm

```



[^1]: No comment

[^2]: Ok, maybe everyone expects this and everything else works this well too, but it's still _wow_

[^3]: [https://console.developers.google.com/](https://console.developers.google.com/)

[^4]: [https://gist.github.com/agam/9f3207a7cdcd72745748](https://gist.github.com/agam/9f3207a7cdcd72745748)
