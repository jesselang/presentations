# Ansible
### Getting started

---

## Ansible is
##### "Automation Made Simple"

* Declarative
* Idempotent
* Agent-less
* Ubiquitous

---

### Declarative & Idempotent

Tell Ansible about "desired state" using YAML.

```
- name: Create a user for John Smith
  user:
    name: jsmith
    uid: 2042
    shell: /bin/zsh
    generate_ssh_key: yes
    state: present
```

Note:
Most modules are *idempotent*. In this example, ansible will check
whether a user called jsmith is present on the system. If not, the
user will be created, otherwise, ansible will let you know that task
is "ok" and not "changed".

---

#### (some of) the same thing in shell

```
id jsmith
if [ $? -ne 0 ]; then
  useradd -u 2042 -s /bin/zsh jsmith
  if [ $? -ne 0 ]; then
    echo "Error: Unable to create user jsmith" >&2
    exit 1
  fi
fi
# TODO: ensure jsmith has uid 2042
chsh -s /bin/zsh jsmith
if [ $? -ne 0 ]; then
  echo "Error: Unable to change shell for jsmith" >&2
  exit 1
fi
# TODO: generate ssh key for jsmith
```

Note: In shell, the commands to perform the single task with Ansible
appear to be more work with lots of checks to ensure nothing went
wrong along the way.

---

### Agent-less

Ansible connects using methods you already know and use.

WinRM on Windows.

SSH on everything else.

Managing existing infrastructure without install investment.

---

### Ubiquitous

On-prem or cloud?

Bare metal or VMs?

Switches, firewalls, routers or SDN?

Containers and microservices, oh my!

Ansible is platform agnostic and has modules to interface with
most of these.

---

### Configuration Management

Consistent infrastructure is hard.

* Hostnames/IPs
* Users/Groups
* File/Device Permissions
* Services/Applications
* QoS/Performance tweaks
* Security Policies/Patches
* Network

Where is the "Source of Truth"?

---

### Enter the Source of Truth

Meet `git`.

```
$ git config --global user.name "John Smith"
$ git config --global user.email jsmith@gitfun.com
$ mkdir infrastructure && cd $_
$ git init
$ echo "localhost" > hosts
$ git add hosts
$ git commit -m "Add a hosts file" hosts
[master (root-commit) 4128963] Add a hosts file
 1 file changed, 1 insertion(+)
 create mode 100644 hosts
```

Note: Meet git. It's great for anything text-based. Source code. Scripts.
YAML. Since Ansible uses YAML, git works great for tracking and sharing
changes. And keeping the source of truth.

---

#### Life is about change
#### and how committed we are to handling it

```
$ echo "lederhosen" > hosts
$ git commit -m "Ocktobkerfest is coming up" hosts
[master 7368551] Ocktobkerfest is coming up
 1 file changed, 1 insertion(+), 1 deletion(-)
```

---

#### History is important, and simple

```
$ git log -1 -p
commit 7368551f501fe2b9d34ae35b80ced1fc3dce7085
Author: John Smith <jsmith@gitfun.com>
Date:   Tue Aug 30 00:50:43 2016 -0500

    Ocktobkerfest is coming up

diff --git a/hosts b/hosts
index 2fbb50c..1707db9 100644
--- a/hosts
+++ b/hosts
@@ -1 +1 @@
-localhost
+lederhosen
```

---

#### Branches: easy to make; easy to merge

```
$ git checkout -b new-hosts
Switched to a new branch 'new-hosts'
$ echo "sparticus" >> hosts
$ echo "zeus" >> hosts
$ git commit -am "Add sparticus and zeus"
[new-hosts 03b352e] Add sparticus and zeus
 1 file changed, 2 insertions(+)
$ git checkout master
Switched to branch 'master'
$ git merge new-hosts
Updating 7368551..03b352e
Fast-forward
 hosts | 2 ++
 1 file changed, 2 insertions(+)
```

Note: master is the default branch in git. Rather than making changes
to master, you can create a branch to make changes to. This offers
a safe place to make and commit changes without affecting the source
of truth, and merge your changes into master when you're ready.

---

#### Inventory

```
mail.example.com

[webservers]
foo.example.com
bar.example.com
two.example.com

[dbservers]
one.example.com
two.example.com
three.example.com
```

```
ansible -m ping webservers
ansible -m ping webservers:dbservers
ansible -m ping 'webservers:&dbservers'
```

---

#### Plays

```
- hosts: webservers
  become: yes
  tasks:
    - name: Install web server
      yum:
        name: nginx
        state: present

    - name: A big welcome
      copy:
        dest: /var/www/index.html
        src: files/index.html
        mode: "0444"
```

---

#### Playbooks

```
- hosts: webservers
  become: yes
  tasks:
    ...

- hosts: dbservers
  tasks:
    ...
```

---

#### Roles

```
- hosts: webservers
  become: yes
  roles:
    - ssh_keys
    - nginx

- hosts: dbservers
  roles:
    - ssh_keys
    - postgres

```

---

#### Variables

```
- hosts: webservers
  become: yes
  vars:
    web_server: apache2
    welcome: index.html
    welcome_mode: "0444"
  tasks:
    - name: Install web server
      yum:
        name: "{{ web_server }}"
        state: present
    - name: A big welcome
      copy:
        dest: "/var/www/{{ welcome }}"
        src: "files/{{ welcome }}"
        mode: "{{ welcome_mode }}"
```

---

#### Variables everywhere

In order of ascending precedence:

* role defaults
* inventory vars
* inventory group_vars
* inventory host_vars
* playbook group_vars
* playbook host_vars
* host facts

...

---

#### Variables everywhere (continued)

Continuing in order of ascending precedence:

* play vars
* play vars_prompt
* play vars_files
* registered vars
* set_facts
* role and include vars
* block vars (only for tasks in block)
* task vars (only for the task)
* extra vars (always win precedence)

---

# Demo Time
