# changing root password

get user info: `racadm getconfig -g cfgUserAdmin -i 1`

set new password: `racadm config -g cfgUserAdmin -o cfgUserAdminPassword -i 2 hunter2`

# enable / disable webserver

get current config: `racadm getconfig -g cfgRacTuning`

disable http: `racadm config -g cfgRacTuning -o cfgRacTuneWebserverEnable 0`

# manage ssh keys

list all keys: `racadm sshpkauth -i 2 -v -k all`

set ssh key: `racadm sshpkauth -i 2 -k 1 -t "ssh-rsa LOREMIPSUMDOLORSITAMET lukas2511@whatever"`

more commands: `racadm help sshpkauth`

# configure serial redirection

```
racadm config -g cfgSerial -o cfgSerialConsoleEnable 1
racadm config -g cfgSerial -o cfgSerialBaudRate 115200
racadm config -g cfgSerial -o cfgSerialCom2RedirEnable 1
racadm config -g cfgSerial -o cfgSerialTelnetEnable 0
racadm config -g cfgSerial -o cfgSerialSshEnable 1
```

# access console

attach: `console com2`

detach: ctrl + alt-gr + \

sometimes first attach doesn't really work (console doesn't react), detach and reattach and it should work

# power control

power off: `stop /system1`

power on: `start /system1`

reset: `reset /system1`

# cheatsheets

http://www.gooksu.com/2015/04/27/racadm-quick-dirty-cheatsheet/





