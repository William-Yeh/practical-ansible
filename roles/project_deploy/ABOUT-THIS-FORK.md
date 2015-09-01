About this fork
===


## Why this fork?

After sending [pull request](https://github.com/f500/ansible-project_deploy/pull/14) to original [f500.project_deploy](https://github.com/f500/ansible-project_deploy) role, there seems some differences in the way we use this role.  To be more convenient for my own usage, I continue developing on this fork.  Of course, I'll try to merge original project's additions along with this fork.


## Differences from original role

Additions:

 - added `project_git_sudo` variable to connect to git with or without out sudo (defaults to `false`); see [GitHub Help](https://help.github.com/articles/error-permission-denied-publickey/) about this.

 - added `project_git_skip` variable to skip the "git clone" step.

 - added `project_owner` and `project_group` variables to set file/directory owner/group, if any.

 - added `project_other_dirs` variable to mkdir, if any.

 - added `project_git_result` and `project_npm_result` registered variables to propagate the `changed` state to the caller.



Original usage remains the same. Read [README](README.md) for details.


## Role Variables

### Optional variables

User-configurable defaults:

```yaml
# use "sudo" mode to connect to git, if absolutely necessary;
# default = False.
project_git_sudo: False


# optionally skip the "git clone" step;
# try to workaround some SSH agent forwarding problems (e.g., on Google Compute Engine).
# default = False.
project_git_skip: False


# set file/directory owner of deployed app;
# default = {{ ansible_ssh_user }}.
project_owner


# set file/directory group of deployed app, if any;
project_group


# array of directories to be `mkdir`, with `project_owner` and `progject_group` set
project_other_dir
```



## Working with *private* git repo

To work with *private* git repo, some details need to be done. See my article “[利用 Ansible 部署 GitHub 專案的設定細節](http://www.codedata.com.tw/social-coding/ansible-github/)” (“HOW-TO: Deploying GitHub Projects with Ansible”) for more information.


If you still encounter strange problems, such as in Google Compute Engine, try the following workaround steps:

1. Set `project_git_skip: true` to skip the role's "git clone" step.
2. SSH into target hosts with agent forwarding enabled: `ssh -A`.
3. Manually "git clone" to `{{ project_source_path }}/shared/source`.
4. Re-run the ansible-playbook.


## License

LGPL v3, same as original project.


## Author Information

William Yeh, william.pjyeh@gmail.com
