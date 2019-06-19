# gcloud client
[![Go Report Card](https://goreportcard.com/badge/github.com/devdinu/gcloud-client)](https://goreportcard.com/report/github.com/devdinu/gcloud-client)

 scripts to do things which you wish google console client does.

## Installation

### Required libs
- [gcloud](https://cloud.google.com/sdk/gcloud) authenticated with projects and config set
- [tmux](https://github.com/tmux/tmux)
- [tmuxinator](https://github.com/tmuxinator/tmuxinator) gem

### Go
`go get -u github.com/devdinu/gcloud-client`

### Homebrew
`brew install devdinu/devlife/gcloud-client`

If you've installed with brew, command will be `gcl`, might need to remove alias from git plugin of oh-my-zsh `~/.oh-my-zsh/plugins/git/git.plugin.zsh`

## Usage

### Refresh
all instances, which's stored in boltd
- `gcloud-client instances refresh`

optionally you can pass flag `--projects proj1,proj2` to refresh specific projects
You could add the refresh as cron

### Search

- `gcloud-client --help` to show the help with flag information

## Add SSH key to compute instances

You could add your ssh key to compute instance[s], so you could ssh directly. default [gcloud compute add ssh](https://cloud.google.com/compute/docs/instances/adding-removing-ssh-keys) overrides the existing keys.
you can add your key along with existing ones in instances with this command.

### Add your ssh key to all google compute instances.
`gcloud-client --limit=10` adds your key to 10 instances

The existing keys with new key is written to temp file and cleared after added to the instance.

You could customize the flags
- `--limit` total instances to add
- `--filter` regexp to filter the instances while listing. uses gcloud `filter=name~'regex'`
- `--user` username to which your ssh key is added for, defaults to `$USER`
- `--ssh_key` ssh_key file to be uploaded, defaults to `$HOME/.ssh/id_rsa.pub`
- `--dbfile` file to store the instances and search, defaults to `$HOME/hosts.db`
- `--projects` list of project-ids to search for while login, or to refresh


```
gcloud-client --ssh_key=$HOME/.ssh/id_rsa.pub --filter='.*pg.*' --limit=10 --user username
```

### Add to single instance
You could give `--instance` and `--zone` to add ssh key to single instance, as its faster than listing instances with regexp

```
gcloud-client --instance=some_instance --zone=asia-zone

```


## CheatSheet
globals flags:
timeout

if you've installed via homebrew, use `gcl` instead of `gcloud-client`

```
gcloud-client ssh grant --key=someone-key.pub --prefix=
gcloud-ciient ssh revoke --name=someone --prefix= # Unimplemented

// searching
gcloud-client instances search --prefix vm-prefix-to-search --project=project
gcloud-client instances search --regex some.*regex # WIP
gcloud-client instances refresh --timeout
gcloud-client instances list --project=specific-project  # WIP

// ssh
gcloud-client instances login --prefix some-prefix --user username --session session_name
gcloud-client instances login --regex some-prefix # WIP
gcloud-client instances login --tag some-prefix   # WIP
```


App:
* [ ] Setup script to install gcl and tmuxinator template and tmuxinator if needed (2.7)
* [ ] CI to build binary
* [X] enable os.Stdin in cmd.execute

SSH ACCESS
* [ ] Display IP after adding the key
* [ ] Adding IP, name mapping to the /etc/hosts file
* [ ] Remove particular ssh key with id
* [ ] Infer id from the sshkey

SEARCH
* [ ] Display progress bar on refresh projects
* [ ] store state (Terminated) information and ignore in search, or show
* [ ] Optimize storing in db performance
    * [X] used goroutines for each project with multiple workers
    * [ ] Write benchmark
* [ ] List projects to use cache
* [X] Override projects from cmdline

CODE/FEATURES
* [X] use logger package
* [X] Define a list of global flags
* [ ] Add gci command to do ls, switch projects
* [ ] Basic Tagging

SIMPLICITY
* [X] use default configuration file, so no need to mention in cmdline

Maybe Laaaaater.
* [ ] Use knife tags to tag instances than manual
