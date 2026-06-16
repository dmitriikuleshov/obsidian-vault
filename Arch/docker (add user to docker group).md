**Create the docker group** (if it doesn't already exist):
```sh
sudo groupadd docker
```
**Add your user to the group**:
```sh
sudo usermod -aG docker $USER
```
**Activate the group changes** immediately for your current terminal session:
```sh
newgrp docker
```
**Test the connection** to make sure it works without `sudo`:
```sh
docker ps
```

hello world