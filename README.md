# flowspec-ff

Op script (that could be converted to an event scipt) to read routes from inetflow.0 and create a JUNOS firewall filter which can then be applied to an interface. In order to get this working you need to be populating the inetflow.0 table with BGP FlowSpec routes from a suitable configured peer. Script is only (currently) reading the destination route /32 and ignoring any other attributes passed through BGP.

```
admin@SRX320> show route table inetflow.0

inetflow.0: 2 destinations, 2 routes (2 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both

8.8.8.8,10.0.0.2/term:1
                   *[BGP/170] 00:21:02, localpref 100, from 192.168.0.126
                      AS path: 65500 I, validation-state: unverified
                      Fictitious
45.45.45.45,10.0.0.2/term:2
                   *[BGP/170] 00:21:02, localpref 100, from 192.168.0.126
                      AS path: 65500 I, validation-state: unverified
                      Fictitious
```                     

Copy the flowspec-ff.slax script to /var/db/scripts/op and configure it's use through JUNOS with the following configuration entry

```
set system scripts op file flowspec-ff.slax
```

Execute the script as follows 

```
op flowspec-ff
```

Firewall filter will look like this...This firewall will then need to be applied to an interface as needed

```
admin@SRX320> show configuration firewall
family inet {
    filter flowspec {
        term dynamic-routes {
            from {
                source-address {
                    8.8.8.8/32;
                    45.45.45.45/32;
                }
            }
            then {
                reject;
            }
        }
        term permit {
            then accept;
        }
    }
}
```


Created as a quick proof of concept that this would work for demonstration. No responsibility taken for large deployments as it has been tested with two routes! 
