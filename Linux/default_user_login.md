# Prompt only the password for a default user in virtual console login

We can use [getty](https://wiki.archlinux.org/title/Getty) to login from a virtual console with a default user, typing the password but without needing to insert the username. For example, let us set the `username` as a default user and prompt its password on `tty1`. First we need to create a [drop-in file](https://wiki.archlinux.org/title/Systemd#Drop-in_files) for the unit `getty@tty1.service`:
```
# systemctl edit getty@tty1.service --drop-in=skip-username
```
This opens the file `/etc/systemd/system/getty@tty1.service.d/skip-username.conf` in your text editor (creating it if necessary) and automatically reloads the unit when you are done editing. Now we add the following lines to the file:
```
[Service]
ExecStart=
ExecStart=-/sbin/agetty -o '-p -- username' --noclear --skip-login - $TERM
```
Don't forget to change *username* properly. Finaly, enable `getty@tty1`
```
# systemctl enable getty@tty1.service
```

## References
- [Getty](https://wiki.archlinux.org/title/Getty)
- [Systemd](https://wiki.archlinux.org/title/Systemd)
