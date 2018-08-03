# Yeah, Got new machine

## Create root user

- Login to your account and open Terminal.
- `sudo passwd root`.
- Type in the new password for UNIX.
- `sudo gedit /usr/share/lightdm/lightdm.conf.d/50-ubuntu.conf`.
- At the end of the file append greeter-show-manual-login = true.
- `reboot`

## Change Computer's hostname

- `sudo hostname katherine`
- `reboot`

## Solve tty caused by root login

https://superuser.com/questions/1241548/xubuntu-16-04-ttyname-failed-inappropriate-ioctl-for-device

- Login as root
- `vim ~/.profile`
- Change last line `mesg n || true` to `tty -s && mesg n || true`

## Install ssh server

- `apt-get install -y openssh-server`
- If can not start, try `tail -f /var/log/auth.log` from server, and `ssh -vvv root@katherine` from client
- ssh by root is disabled by default, you should set `PermitRootLogin yes` then `service ssh restart`

## Install Synergy

- `dpkg -i /path/to/synergy.deb`
- Start by `/usr/bin/synergyc -f --yscroll 56 --no-tray --name katherine 192.168.222.90:24800`
- Press `windows` and search `Startup Applications`
- Add it and it will save into `~/.config/autostart/` directory

## Install Google Chrome

- `dpkg -i /path/to/google_chrome.deb`
- Run `google-chrome --no-sandbox` to let it running as root

## Install Sougou Input

Sougou input is rely on `fcitx`.

- Enter `Setting`->`Language Support`->`Keyboard input method system`
- If no `fcitx`, you need install it
    - `sudo add-apt-repository ppa:fcitx-team/nightly`
    - `apt-get update`
    - `apt-get install fcitx`

## Remap Key

- Find keycode by `xev`, If you got `unable to open display ''`, please use `DISPLAY=:0 xev`
- ??

# CCMS

`apt-get install compizconfig-settings-manager`

# Advance usage

press Advanced Search >, enable the checkbox for searching in setting values, and do a search for <Super>p (and <Mod4>p, just in case -- both spellings appear to work).

## Enable Mission Control like Mac

- Launch CCSM, *window manager->scale->bindings*

## Disable Super Key Launcher

- *Ubuntu Unity Plugin->Launcher*
- Key to show the Dash, Launcher and Help Overlay

## Disable Super+P to switch Display

- `dconf write /org/gnome/settings-daemon/plugins/xrandr/active false`

# Screenshot

- *Setting->Keyboard->Shortcuts->Custom Shortcuts*
- Add `gnome-screenshot -a`

# Change default editor to vim

- `sudo update-alternatives --config editor`

# Show git branch at terminal

```shell
parse_git_branch() {
 git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}
PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u\[\033[00m\]:\[\033[01;34m\]\W\[\033[01;31m\]$(parse_git_branch)\[\033[00m\]\$ '
```

- Add above script to `~/.bashrc`
- Run `source ~/.bashrc`
