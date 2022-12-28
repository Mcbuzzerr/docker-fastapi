# FastAPI + Beanie + Docker + GitLab

A repository with a FastAPI app and CI/CD enabled with a self-hosted GitLab container.

## Development

```bash
docker-compose up -d
```

## Test

The tests can be found in `api/test_main.py`.

Set the `TEST_MONGO_URI` in the environment if you are not using `localhost`. Then just run the test suite. The `-s` flag enables printed output.

```bash
docker exec -it docker-fastapi pytest -s
```

To test a specific function from `api/test_main.py`, use the `-k` option.

```bash
docker exec -it docker-fastapi pytest -s -k "test_comments"
```

## CI/CD

Docker In Docker (dind)
GitLab running in dind

### Self host GitLab

> NOTE: The GitLab container takes like 10-15 minutes to get up and running. Check the logs to see when it's ready.

https://docs.gitlab.com/ee/install/docker.html#install-gitlab-using-docker-compose

### Initial Login to GitLab

Username: root

Get the password:

```bash
# gitlab is the name of the container
docker exec -it gitlab bash -c 'grep "Password:" /etc/gitlab/initial_root_password'
```

### Create New Project on GitLab

1. Deactivate new user registration when prompted (recommended).
2. Add SSH Key http://localhost:8980/help/user/ssh.md#add-an-ssh-key-to-your-gitlab-account.

```bash
# create blank project
git push --set-upstream ssh://git@localhost:8922/root/$(git rev-parse --show-toplevel | xargs basename).git $(git rev-parse --abbrev-ref HEAD)
# add remote to git repo
git remote add gitlab ssh://git@localhost:8922/root/$(git rev-parse --show-toplevel | xargs basename).git
# deploy
git push -u gitlab
```