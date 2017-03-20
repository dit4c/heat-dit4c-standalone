# Run DIT4C container images with OpenStack Heat

[DIT4C][dit4c] has a lot of advantages for running research tools in the cloud. However, it also comes with a central administration cost. Wouldn't it be to run a DIT4C container (which is just a normal Docker/AppC container that exposes all its functionality via HTTP) standalone in the cloud without needing DIT4C?

This OpenStack Heat template allows you to run a DIT4C container by itself on a OpenStack Nova instance, allowing you to get some of the positives of DIT4C without needing a centrally-administered service. It uses [ngrok2-relay][ngrok2-relay] & [password-reverse-proxy][password-reverse-proxy], forked from DIT4C helper images, to expose and protect a DIT4C container instance.

As a starting point, it requires a CoreOS image to be available in OpenStack Glance. If you don't have one already, you [can upload one yourself][coreos-openstack].

The template takes the following inputs:

 * Instance Size - how big would you like the instance running the container to be?
 * Image ID - the Glance ID of your CoreOS image
 * Availability Zone - where should the instance be provisioned?
 * ngrok Region - the instance uses [ngrok.com][ngrok.com] to expose itself to the world. Pick a region that's closest to you.
 * Container Image - the container image to run. Can be Docker (`docker://mydockerhubaccount/mydockerrepo`) form, or any other form [rkt][rkt] accepts.
 * Container Port - which port does the container expose its HTTP functionality on? For all standard DIT4C containers, this is likely `8080`.
 * Password - your container is protected by a single fixed password prompt. Choose wisely.
 * Notify URL - Reading the instance logs to get the ngrok.com URL for the container can be tiresome. Include a valid URL here and the URL will be POST`ed when it's ready. [dweet.io][dweet] works well for this.
 * Key Name - you'll likely want an SSH key for the host, even if you don't plan to use it, in case your VM gets rebooted/or and you want to save your instance via `rkt export`.

Note that because the instance is exposed via [ngrok.com][ngrok.com], it doesn't require a public IP address or any open ingress ports.

Obviously, this is a far cry from [DIT4C][dit4c] itself, with its support for OAuth & saving of instances to images through the web UI. However, if you don't have the time/resources to run DIT4C, this might be enough for the work you need to get done.

[dit4c]: https://dit4c.github.io/
[coreos-openstack]: https://github.com/coreos/scripts/tree/master/oem/openstack
[ngrok.com]: https://ngrok.com/
[rkt]: https://coreos.com/rkt/
[dweet]: https://dweet.io/
[ngrok2-relay]: https://github.com/dit4c/ngrok2-relay
[password-reverse-proxy]: https://github.com/dit4c/password-reverse-proxy
