# salt-gitfs-containerized

1. Clone this repo:
  ```
  git clone https://github.com/thebluesnevrdie/salt-gitfs-containerized.git /srv/salt-gogs
  ```
1. Generate deploy keys for the Salt Master:
  ```
  cd /srv/salt-gogs/salt-master/root-ssh/
  ssh-keygen -t ecdsa -b 521 -C "salt@example.org" -f ./id_ecdsa -N ''
  ```
1. Ensure UID/GID are set correctly for our volumes:
   ```
   chown -R 999:999 /srv/salt-gogs/*
   chown -R root:root /srv/salt-gogs/salt-master/root-ssh
   ```
1. Make Postgres happy:
   ```
   rm /srv/salt-gogs/gogs-db/data/.gitkeep
   ```
1. Bring up the containers:
  ```
  docker-compose up -d gogs gogs-db ; docker-compose logs -f
  ```
1. Register in Gogs
1. Create Org and repository
1. Test SSH connectivity to Gogs:
```
ssh -i /srv/salt-gogs/salt-master/root-ssh/id_ecdsa -p2222 -T gogs@gogs
```
1. Push sample repo to Gogs
```
git remote add origin ssh://gogs@gogs:2222/ops/config-mgmt-test.git
git push -u origin master
```
1. Add deploy key for Salt Master
```
cat /srv/salt-gogs/salt-master/root-ssh/id_ecdsa.pub
```
1. Bring up Salt Master container
```
docker-compose up -d ; docker-compose logs -f
```
1. Test Salt Master:
```
docker-compose exec salt-master salt '*'
```
