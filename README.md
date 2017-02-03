# flowspec-ff

Op script (that could be converted to an event scipt) to read routes from inetflow.0 and create a JUNOS firewall filter which can then be applied to an interface

'''
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
'''                      
