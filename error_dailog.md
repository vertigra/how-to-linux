# debconf: (No usable dialog-like program is installed, so the dialog based frontend cannot be used)
*OC: Debian 8 Jessie*


**Симптомы**:
```bash
# apt-get install some_program
debconf: unable to initialize frontend: Dialog
debconf: (No usable dialog-like program is installed, so the dialog based frontend cannot be used. at /usr/share/perl5/Debconf/FrontEnd/Dialog.pm line 76.)
debconf: falling back to frontend: Readline
```
**Решение**:
```bash
apt-get install dialog
```
или так:
```bash
apt-get install whiptail
```