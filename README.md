# ansible-role-clone-repo

A role for cloning public and private github repos to a remote machine


## Requirements

Ansible version 2.1+

### Agent Forwarding

To clone a private repo you must have your ssh agent running and agent forwarding enabled for this role to work. The agent is how the remote machine will authenticate with github.

To see if your ssh agent is running, run:
```bash
ssh-add -l
```
If you see:
```bash
The agent has no identities.
```
Then your agent has no identities added to it. If you want the remote machine to authenticate via your host's `id_rsa` file, then run:
```bash
ssh-add ~/.ssh/id_rsa
```
Now if you run `ssh-add -l` again, you'll see something like:
```bash
2048 SHA256:utfaksjdhfgaksjdhfgkasjhdfgkasjhdfgkasjaksj /Users/adamyala/.ssh/id_rsa (RSA)
```

#### Vagrant

If you're using this with a Vagrant machine, below is an example config for provisioning that includes agent forwarding. The `Agent Forwarding` step above still must be done in addition to adding this configuration.
```ruby
config.ssh.forward_agent = true
config.vm.provision :ansible do |ansible|
  ansible.playbook = 'your_playbook_here.yml'
  # If running on ubuntu16, uncomment the below line
  # ansible.raw_arguments = '-e ansible_python_interpreter=/usr/bin/python3'
end
```


## Role Variables

| Name| Description | Required/Optional | Example |
| ---------------- | -------------------------------------------------------------------- | ------- | ------- |
| `gh_repo` | The `Clone with SSH` url to the repo | (required) | `git@github.com:githubtraining/hellogitworld.git` |
| `gh_branch` | The branch of the repo to checkout | (optional) | `master` |
| `dest_dir` | The destination directory of the repo. The repo dir will be named after the last path dir in path provided. In the example the repo dir will be named `foo`. | (required) | `/www/foo` |
| `dest_dir_owner` | The final owner of the repo dir. Recursively set. | (required) | `www-data` |
| `dest_dir_group` | The final group of the repo dir. Recursively set. | (required) | `www-data` |
| `dest_dir_perm` | The final permissions set on the repo dir  | (required) | `0755` |


## Dependencies

None


## Example Playbook

```yaml
- hosts: someserver
  roles:
    - role: ansible-role-clone-repo
      gh_repo: git@github.com:githubtraining/hellogitworld.git
      dest_dir: /www/foo
      dest_dir_owner: www-data
      dest_dir_group: www-data
      dest_dir_perm: 0755
```

## License

MIT


## Author Information

[Adam Yala](https://github.com/adamyala)
