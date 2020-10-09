# 50,000 Kubernetes Jobs for 50,000 Subscribers

[![CI](https://github.com/geerlingguy/50k-k8s-jobs/workflows/CI/badge.svg)](https://github.com/geerlingguy/50k-k8s-jobs/actions?query=workflow%3ACI)

I just passed 50,000 subscribers on my YouTube channel, so I thought I'd find a fitting way to celebrate.

For 10,000 subscribers, I ran 10,000 K8s Pods on an Amazon EKS cluster; you can find a video detailing that here: [10,000 Pods for 10k Subscribers](https://www.youtube.com/watch?v=k5ncj3TKL1c).

So for 50,000, I was doing a few calculations and found that I'd have to pay a few hundred bucks (at least!) just for the time on an EKS cluster with enough workers to run 50,000 Pods simultaneously. So that was right out.

Instead, I'm building this Ansible playbook to automate the process of running 50,000 individual Jobs on a Kubernetes cluster!

## Local Testing

Make sure you have [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) and [Kind](https://kind.sigs.k8s.io/docs/user/quick-start/) installed, then run the following:

  1. `kind create cluster`
  1. `pip3 install ansible molecule[docker] yamllint ansible-lint openshift`
  1. `ansible-galaxy install -r requirements.yml`
  1. `molecule converge`

When you're finished, run `kind delete cluster`.

## Running on a Production Cluster

I guess this is a great opportunity to thank this project's sponsor, Linode; they not only sponsored this video, but they also did two other very kind things:

  1. They gave me some credit to try out this project on one of their Kubernetes clusters.
  2. They gave me a special link I can share with you to get a **$100 60-day free credit** on your own new Linode account!

Seriously! Go [try out Linode using my link](https://www.linode.com/geerling), and you can take some new infrastructure for a spin for free!

Anyways, since I knew I'd build this cluster on Linode, I went ahead and did the following:

  1. Created my Linode account (use [this link](https://www.linode.com/geerling) for free credit!).
  2. Created a new Kubernetes Cluster.
  3. Added 10 8GB Linodes to the Cluster.
  4. Waited for the Cluster and all Nodes to boot.
  5. Downloaded the Kubeconfig file from Linode's Kubernetes UI.
  6. Told Ansible about the Kubeconfig with: `export K8S_AUTH_KUBECONFIG=~/.kube/linode.yaml`
  7. Ran `molecule converge`

That runs with all the defaults in the playbook. If you want to override them, the easiest way is to run `ansible-playbook` and pass extra variables:

    ansible-playbook main.yml --extra-vars="{'batch_size':25,'total_count':10000}"

Check on how many jobs have completed by monitoring:

    kubectl get jobs -l type=50k --field-selector status.successful=1
