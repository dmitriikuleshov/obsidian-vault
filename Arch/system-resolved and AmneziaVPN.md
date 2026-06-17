Connection drops and DNS resolution issues with AmneziaVPN on Arch Linux are typically caused by missing `systemd-resolved` dependencies, or routing collisions with `NetworkManager`

Amnezia heavily relies on `systemd-resolved` to route and resolve DNS securely. Make sure it is active:

```sh
sudo systemctl enable --now systemd-resolved
```

Ensure your `/etc/resolv.conf` is properly linked to the systemd stub:

```sh
sudo ln -sf /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
```