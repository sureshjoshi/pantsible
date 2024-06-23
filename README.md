# pantsible

[Ansible](https://www.ansible.com/) with an embedded Python Interpreter... Also, this project uses [Pants](https://pantsbuild.org)...

## Why?

To solve a mildly annoying chicken and egg problem.

When I provision a Mac, I want to use my Ansible playbooks to do it. So I need to [install Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html), but to install Ansible, I need to have [pipx](https://pipx.pypa.io/stable/installation/) installed. To install `pipx` I need to have [homebrew](https://brew.sh) installed.

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

## How to Build

- Install [Pants](https://www.pantsbuild.org/2.21/docs/getting-started/installing-pants)
- `pants package :pantsible`

## How to Run

Whether you build the binary it locally, or download it from Releases - you should probably put the binary in a directory on your `PATH` (e.g. `/usr/local/bin`, `~/.local/bin`, etc) and maybe re-name it if you're so inclined.

Since the Ansible CLI is built up of subcommands (each command is a small Python shim calling the appropriate module), you must select one. Running the bare `pantsible` will net you:

```bash
% pantsible
Error: Could not determine which command to run.

Ansible with an embedded Python interpreter.

Please select from the following boot commands:

ansible
ansible-config
ansible-console
ansible-doc
ansible-galaxy
ansible-inventory
ansible-playbook
ansible-pull
ansible-vault

You can select a boot command by setting the SCIE_BOOT environment variable or else by passing it as the 1st argument.
```

In other words, `pantsible` is a [BusyBox](https://www.busybox.net/); so you need to structure commands like:

```bash
pantsible ansible-vault encrypt <file>

# or, more verbosely:

SCIE_BOOT=ansible-vault pantsible encrypt <file>
```

Neither of these approachs is very appealing. Instead, you can one-time install aliases for all the subcommands with `SCIE=install pantsible --symlink DEST_DIR`, where `DEST_DIR` can be any element of your `PATH` you desire (e.g. `/usr/local/bin`, `~/.local/bin`, etc...). See `SCIE=help pantsible` for more info.

After installing the symlinks, we've replicated the [Ansible CLI](https://docs.ansible.com/ansible/latest/command_guide/index.html) and you can use the regular commands:

```bash
ansible --version
ansible-playbook --version
ansible-vault --version
...
```
