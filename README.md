# jenkins-gitlab-pipeline
jenkins-gitlab-pipeline

### Use below instructions to create the gitlab custom hooks.

```
1) Pick a project that needs a custom Git hook.
2 )On the GitLab server, navigate to the project's repository directory. For an installation from source the path is usually /home/git/repositories/<group>/<project>.git. For Omnibus installs the path is usually /var/opt/gitlab/git-data/repositories/<group>/<project>.git.
3) Create a new directory in this location called custom_hooks.
4) Inside the new custom_hooks directory, create a file with a name matching the hook type. For a pre-receive hook the file name should be pre-receive with no extension.
5) Make the hook file executable and make sure it's owned by git.
```
