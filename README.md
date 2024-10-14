# efk_on_k3s_vagrant_libvirt_ansible

Vagrant-libvirt setup that creates a VM with [k3s](https://k3s.io/), the minimal
lightweight Kubernetes distribution.

On top of k3s, this setup installs [the EFK stack (Elasticsearch, Fluentd and
Kibana) using the ECK
operator](https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-deploy-eck.html).
It also installs an instance of Nginx and a MinIO instance, just to have some
workload in the cluster.

There is a second branch called `fluentbit` that uses [Fluent
Bit](https://fluentbit.io/) instead of Fluentd for collecting the logs.

Default OS is openSUSE Leap 15.6, but that can be changed in the Vagrantfile.
Please be aware, that this might break the Ansible provisioning.

## Vagrant

1. You need `vagrant`, obviously. And `git`. And Ansible...
1. Fetch the box, per default this is `opensuse/Leap-15.6.x86_64`, using
   `vagrant box add opensuse/Leap-15.6.x86_64`.
1. Make sure the git submodules are fully working by issuing
   `git submodule init && git submodule update`
1. Run `vagrant up`
1. Open the URL that was printed out at the end of the Ansible provisioning. It
   should look something like `http://kibana.192.0.2.13.sslip.io`. Use the
   `elastic` user and the password `totallynotsecurE1@`.
1. Dismiss the "Start by adding integrations" popup by clicking on `Explore on
   my own`. Click on **Discover** in the menu on the left, click on **Create
   data view**. Add `logstash` as name and `logstash-*` as **Index pattern**.
   You should see logs appear, collected by Fluentd from your pods, sent to
   Elasticsearch and shown by Kibana.
1. Party!

## Cleaning up

The VMs can be torn down after playing around using `vagrant destroy`. This will
also remove the kubeconfig file `ansible/k3s-kubeconfig`.

## License

BSD-3-Clause

## Author Information

I am Johannes Kastl, reachable via git@johannes-kastl.de
