# koesterlab-workstation-ansible

This ansible playbook configures the workstations in koesterlab to become a usable working environment.
It assumes that Fedora Linux and ansible (`sudo dnf install ansible`) has been already installed.
If that is not already the case take a look at the [prerequisites](PREREQUISITES.md)

## Setup

### Step 1: SSH keys

You have to create two SSH keys. One for the IKIM cluster, one for Git (unless you already have them).
To do this, run:

```
ssh-keygen -t ed25519 -f ~/.ssh/id_ikim
ssh-keygen -t ed25519 -f ~/.ssh/id_git
```

In both cases, make sure to provide a secure, unique password when prompted.

### Step 2: Storing configuration

You have to store all required information for the workstation configuration under
`~/.config/ansible/vars.yml`. Create the file as follows

```bash
mkdir -p ~/.config/ansible
touch ~/.config/ansible/vars.yml
```

Then open the file and enter the following content, replacing the values with your own:
```yaml
ikim_username: johannes
ikim_ssh_key: id_ikim
git_ssh_key: id_git
git_email: johannes.koester@uni-due.de
git_name: Johannes Koester
```

### Step 3: Clone the playbook repository

```sh
cd /tmp
git clone https://github.com/koesterlab/koesterlab-workstation-ansible
cd koesterlab-workstation-ansible
```

### Step 4: Execute the ansible playbook to configure your workstation

Execute the configuration with

```bash
ansible-playbook -K playbook.yml
```

You will be asked for the `BECOME password`. This is simply your user password.
In order to adapt to changes in the workstation setup, simply rerun the playbook with the same command.
Ansible will automatically only execute the changes.


### Step 5: Optionally enable fractional scaling of fonts (to get larger font sizes)

This is still experimental in linux, and therefore should be only enabled if you need it to be able to properly read.
Instructions can be found [here](https://www.linuxfordevices.com/tutorials/centos/enable-fractional-scaling-fedora).
