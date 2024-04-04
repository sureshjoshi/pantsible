# pansible

Ansible with an embedded Python Interpreter... Also, this project uses [Pants](https://pantsbuild.org)...

## Why?

To solve a mildly annoying chicken and egg problem.

When I provision a Mac, I want to use my Ansible playbooks to do it. So I need to [install Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html), but to install Ansible, I need to have [pipx](https://pipx.pypa.io/stable/installation/) installed. To install `pipx` I need to have `homebrew` installed.

But I want my Ansible playbook to install `homebrew` for me.

I could just use the system Python interpreter (which is usually years out of date) to install Ansible to the global site-packages, but I never do that:

```bash
export PIP_REQUIRE_VIRTUALENV=true
```

So, I guess I could create a temp venv, activate it, install Ansible, run my playbook, delete the venv, and then re-install Ansible using either `pipx` or `homebrew`.

...

OR, I could take 20 minutes to create this project which bundles Ansible with a Python interpreter - and then `curl` it down to my Mac and run it.

## How to Upgrade Ansible

Update the `requirements.txt` with the version of interest.
