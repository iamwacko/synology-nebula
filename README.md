# Nebula Mesh Network On Synology

Nebula is like a VPN that connects computers together. No ports need to be opened, it can NAT punch just fine.

## Installation Steps

1. In the DSM Web GUI, open the Control Panel, “Shared Folder” ↦ “Create” ↦ ”Create Shared Folder”, name it `Nebula` 
2. Do not enable encryption nor checksums when creating folder
3. Remove all permissions for every current user, a new user is next
4. create a user using the DSM GUI. Control Panel, “User & Group” ↦ “Create” and set nebula as the username 
5. The password should be long and random, but will not be needed later, and no application or service permissions are needed
6. Give the user all of the rights on the new shared folder
7. Download the installer: run `curl -L https://github.com/malt3/synology-nebula/archive/refs/tags/v1.0.0.tar.gz | tar xz --strip-components=1` in the shared folder
8. Get Nebula configuration file (generate one if it's your own mesh, or ask if you aren't admin), and place it at `config/config.yml` in the shared folder
9. Run the installer. `sudo sh install.sh`
10. `ip a show dev nebula` should show a VPN interface
11. DSM Web GUI, Control Panel, “Task Scheduler” ↦ “Create” ↦ ”Triggered Task” ↦ “User-defined script”. 
12. Pick any name, choose the root user, and select Boot-up as Event. Under “Task Settings”, enter the following for the user-defined script:

```
cp /volume1/Nebula/systemd/nebula.service /etc/systemd/system/
systemctl daemon-reload
systemctl enable nebula
systemctl start nebula
```

And now it's installed! It'll keep running, and will persist through reboots.
