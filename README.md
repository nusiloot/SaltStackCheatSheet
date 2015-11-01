# SaltStackCheatSheet

## Installing SaltStack - Ubuntu 14.*

```
wget -O - https://repo.saltstack.com/apt/ubuntu/ubuntu14/latest/SALTSTACK-GPG-KEY.pub | sudo apt-key add -
echo 'deb http://repo.saltstack.com/apt/ubuntu/ubuntu14/latest trusty main' | sudo tee -a /etc/apt/sources.list
sudo apt-get update

# Master installation
apt-get install salt-master

# Minion installation
apt-get install salt-minion

# Salt ssh installation
apt-get install salt-ssh

# Salt syndic installation
apt-get install salt-syndic
```

## Debugging

```
# Debugging the master
salt-master -l debug

# Debugging the minion
salt-minion -l debug
```

## SaltStack Documentation

```
# Viewing all the documentation
salt '*' sys.doc

# Viewing a module documentation
salt '*' sys.doc module_name

#Examples:
salt '*' sys.doc status
salt '*' sys.doc pkg     
salt '*' sys.doc network 
salt '*' sys.doc system
salt '*' sys.doc cloud

# Viewing a function documentation
salt '*' sys.doc module_name function_name

# Examples:
salt '*' sys.doc  auth django
salt '*' sys.doc sdb sqlite3
```

## SaltStack Modules And Functions:

```
salt '*' sys.list_modules
salt '*' sys.list_functions
```

## Compound matchers:

| Letter | Match Type | Example | Alt Delimiter?] |
| --- | --- | --- | --- |
| G | Grains glob | G@os:Ubuntu | Yes |
| E | PCRE Minion ID | E@web\d+\.(dev\|qa\|prod)\.loc | No |
| P | Grains PCRE | P@os:(RedHat\|Fedora\|CentOS) | Yes |
| L | List of minions | L@minion1.example.com,minion3.domain.com or bl*.domain.com | No |
| I | Pillar glob | I@pdata:foobar | Yes |
| J | Pillar PCRE | J@pdata:^(foo\|bar)$ | Yes |
| S | Subnet/IP address | S@192.168.1.0/24 or S@192.168.1.100 | No |
| R | Range cluster | R@%foo.bar | No |

Other examples: 

```
# Examples taken from: https://docs.saltstack.com/en/latest/topics/targeting/compound.html

# Joining
salt -C 'webserv* and G@os:Debian or E@web-dc1-srv.*' test.ping
salt -C '( ms-1 or G@id:ms-3 ) and G@id:ms-3' test.ping

# Excluding
salt -C 'not web-dc1-srv' test.ping
```

## Upgrades

```
#
# Listing upgrades
salt '*' pkg.list_upgrades

# Upgrading
salt '*' pkg.upgrade
```

## Reboot And Uptime

```
# Reboot
salt '*' system.reboot

#Uptime
salt '*' status.uptime
```

## Grains

```
# Syncing grains
salt '*' saltutil.sync_grains

# Available grains can be listed by using the ‘grains.ls’ module:
salt '*' grains.ls

# Grains data can be listed by using the ‘grains.items’ module:
salt '*' grains.items

# Grains have values that could be called via ‘grains.get <grain_name>’ (path is the name of a grain)
salt '*' grains.get path
```

## Syncing Data

```
# Syncing grains
salt '*' saltutil.sync_grains

# Syncing everything from grains to modules, outputters, renderers, returners, states and utils.
salt '*' saltutil.sync_all
```

## Running System Commands

```
salt "*" cmd.run "ls -lrth /data"
salt "*" cmd.run "df -kh /data"
salt "*" cmd.run "du -sh /data"
```

## State Declaration Structure

```
# Source: https://docs.saltstack.com/en/latest/ref/states/highstate.html#state-declaration

# standard declaration
<ID Declaration>:
  <State Module>:
    - <Function>
    - <Function Arg>
    - <Function Arg>
    - <Function Arg>
    - <Name>: <name>
    - <Requisite Declaration>:
      - <Requisite Reference>
      - <Requisite Reference>


# inline function and names
<ID Declaration>:
  <State Module>.<Function>:
    - <Function Arg>
    - <Function Arg>
    - <Function Arg>
    - <Names>:
      - <name>
      - <name>
      - <name>
    - <Requisite Declaration>:
      - <Requisite Reference>
      - <Requisite Reference>


# multiple states for single id
<ID Declaration>:
  <State Module>:
    - <Function>
    - <Function Arg>
    - <Name>: <name>
    - <Requisite Declaration>:
      - <Requisite Reference>
  <State Module>:
    - <Function>
    - <Function Arg>
    - <Names>:
      - <name>
      - <name>
    - <Requisite Declaration>:
      - <Requisite Reference>
```
