This is the Anchor configuration management version of Jinja2 project using Mustache, JSON and TEMPLATE command of Anchor CM.

Anchor CM template (test.tmpl)
==============================

```
conf
#hostname config
hostname {{hostname}}

# ntp server config

{{#ntpServer}}
ntp server {{ip}}
{{/ntpServer}}

#vlan config
{{#vlans}}
vlan {{id}}
	name {{name}}
{{/vlans}}

#default gateway
{{#misc}}
ip route 0.0.0.0 0.0.0.0 {{defaultgw}}

snmp-server user {{snmp.user}} 
snmp-server host {{snmp.trap.trapServer}} version {{snmp.trap.version}}
{{/misc}}
```

JSON (test.json)
================

```javascript
{
    "hostname": "testroute",
    "ntpServer": [
        {
            "ip": "10.1.1.1"
        },
        {
            "ip": "11.1.1.1"
        }
    ],
    "vlans": [
        {
            "id": "100",
            "name": "accessvlan"
        },
        {
            "id": "200",
            "name": "trunkvlan"
        }
    ],
    "misc": {
        "defaultgw": "40.1.1.1"
    },
    "snmp": {
        "user": "admin",
        "trap": {
            "trapServer": "1.1.1.1",
            "version": "3"
        }
    }
}
```

Anchor cmdfile.txt
===================

```
[code]

# TEMPLATE template file, json file, destination file, shell script file

TEMPLATE test.tmpl test.json testconfig.txt testecho.bat
```

Run Anchor cmdfile
==================

anchor cmdfile.txt

Notify script (testecho.bat)
============================

```
echo "Success"
```

Output of testconfig.txt
========================

```
conf
#hostname config
hostname testroute

# ntp server config

ntp server 10.1.1.1
ntp server 11.1.1.1


#vlan config
vlan 100
	name accessvlan
vlan 200
	name trunkvlan


#default gateway
ip route 0.0.0.0 0.0.0.0 40.1.1.1

snmp-server user admin 
snmp-server host 1.1.1.1 version 3
```
