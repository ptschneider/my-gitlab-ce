# my-gitlab-ce

pull and use the gitlab/gitlab-ce image directly,

docker-compose example relies on local registry for deployment, does not pull directly from internet

this setup assumed simple user/password authentication on secure intranet, small group no external contributors


export $(cat env.txt)

was using GITLAB_HOME=/work/pschneider/gitlab/local_gitlab_home

run 'docker-compose up -d' and wait a few minutes, it churns a bit

then run the following to get the root password

docker exec -it gitlab-ce grep 'Password:' /etc/gitlab/initial_root_password


$ docker exec -it gitlab-ce grep 'Password:' /etc/gitlab/initial_root_password
Password: mXiI4qI/lenytFLKCCMj82yO/icwRtXKUyHc3g3Vvks=
```

username: root

now, connect to the running instance with a web browser and login as root

this will allow you to approve other accounts

skip email integration; have users register with bogus domains, use root account to manually approve login

have a ssh key pair generated for your user account, you can add your public key right away and use ssh


after a bit

then, you'll notice gitlab-runner is fouled up
error about missing a .toml file
search and find it is really a token registration problem
for a token feature that was deprecated anyway
wtf

to get the token, the broken system has to be up and running
you navigate to http:<yer_gitlab_ce>/admin/runners
click on the link about deprecated tokens
and you get the instructions below:

```
# Download the binary for your system
sudo curl -L --output /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64

# Give it permission to execute
sudo chmod +x /usr/local/bin/gitlab-runner

# Create a GitLab Runner user
sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash

# Install and run as a service
sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
sudo gitlab-runner start

sudo gitlab-runner register --url http://docker01mxlinux/ --registration-token stkW-s5KzGGVC1fFfPsP
```

create a project


## Container Registry

Here, just talking about the 'classic' gitlab support for integrated container registry, not the 'next generation' stuff.

Gitlab supports per-project registry creation/management with integrated security.

IME, this is overkill for a self-hosted gitlab instance that is only supporting a few related projects.

I leave it disabled, and have an external local registry instead.

