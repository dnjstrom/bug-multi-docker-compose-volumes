# Forwarding volumes through multiple layers of Docker Compose

In a CI-setup I'm working on we're using kubernetes to run a docker container which runs docker-compose which is performing end-to-end testing. As I was working on this setup, I noticed I couldn't mount the filesystem through to the innermost container. This repo is a minimal reproduction of the issue using only docker-compose.

Tested on:

- Docker for Mac: 18.03.1-ce-mac65 (24312)
- Engine: 18.03.1-ce
- Compose: 1.21.1

## How to try it yourself

```bash
git clone https://github.com/dnjstrom/bug-multi-docker-compose-volumes.git
cd bug-multi-docker-compose-volumes
docker-compose run --rm test
```

## My output

The files in `/outer` and `/inner` should theoretically be the same, but they're not.

```bash
❯ docker-compose run --rm test
Creating network compose_default with the default driver
Outer container:
/outer
total 16
drwxr-xr-x    6 root     root           192 Jul 17 16:06 .
drwxr-xr-x    1 root     root          4096 Jul 17 16:17 ..
-rw-r--r--    1 root     root           158 Jul 13 14:16 README.md
-rw-r--r--    1 root     root           292 Jul 17 16:13 docker-compose.test.yml
-rw-r--r--    1 root     root           168 Jul 17 16:17 docker-compose.yml
Inner container:
/inner
total 4
drwxr-xr-x    2 root     root            40 Jul 17 16:13 .
drwxr-xr-x    1 root     root          4096 Jul 17 16:17 ..
```
