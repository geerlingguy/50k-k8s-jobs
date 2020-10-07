# 50,000 Kubernetes Jobs for 50,000 Subscribers

I just passed 50,000 subscribers on my YouTube channel, so I thought I'd find a fitting way to celebrate.

For 10,000 subscribers, I ran 10,000 K8s Pods on an Amazon EKS cluster; you can find a video detailing that here: [10,000 Pods for 10k Subscribers](https://www.youtube.com/watch?v=k5ncj3TKL1c).

So for 50,000, I was doing a few calculations and found that I'd have to pay a few hundred bucks (at least!) just for the time on an EKS cluster with enough workers to run 50,000 Pods simultaneously. So that was right out.

Instead, I'm building this Ansible playbook to automate the process of running 50,000 individual Jobs on a Kubernetes cluster!

## Local Testing

Make sure you have [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) and [Kind](https://kind.sigs.k8s.io/docs/user/quick-start/) installed, then run the following:

  1. `kind create cluster`
  1. `pip3 install ansible molecule[docker] yamllint ansible-lint openshift`
  1. `ansible-galaxy install -r requirements.yml`
  1. `molecule test`

When you're finished, run `kind delete cluster`.

## Running on a Production Cluster

TODO.
