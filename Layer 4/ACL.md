# Access Control List

<style>body {text-align: justify}</style>

ACL are used to control the access to the network. They are used to restrict or allow the access to the network to certain devices, protocols, and MAC and IP addresses (traffic filtering) but also to implement routing policies (Policy Based Routing), to categorize the traffic (classification) and even to debug traffic based on ACL. ACLs and firewalls are two different concepts but can sometimes be used interchangeably. The main difference is that firewalls are used to inspect sessions and can analyse up to layer 7 (application layer) whereas ACLs are used to inspect layer 4 (transport layer). Firewalls can also be used for intrusion detection and prevention (IDS). Also ACL can be deployed on switches and routers, whereas firewalls are specific hardware and software. Even if ACL are now less and less used to the detriment of firewalls and security groups, they can still be useful in some cases.

Frequent issues :

- [Implicit deny](#implicit-deny)
- [Incorrect order](#incorrect-order)
- [ACL missconfiguration](#acl-missconfiguration)
- [Badly chosen ACL](#badly-chosen-acl)
- [ACL port missconfiguration](#acl-port-missconfiguration)

## Implicit deny

[//]: <> (To Do)

This might give a smile to networking veterans, but this is indeed a very frequent mistake and always the first one I check. The implicit deny is the default behavior of the ACLs, when no other rules are matched.

### Symtoms

All traffic except the ones that are explicitly allowed is denied.

> ⚠️ If you made a rule that block some traffic and have a allow all in the end, you will not face this issue but this is usually a bad practice... ACL is much more pertinent when used only to allow specific traffic and deny everything else.

### Diagnostics

Cisco IOS don't record the number of hits for the implicit deny. The best way to check how impactful the implicit deny is is to add an explicit deny rule that will supersede the implicit deny.

[//]: <> (You can also use the command `show ip access-list access-list-number | access-list-name] extensive` to check the ACLs hit count and the hit details.)

You can use the command `show ip access-list [access-list-number | access-list-name]` to check the ACLs hit count.
You can use the command ` show ip access-list interface interface-name [in| out]` to check the ACLs hit count on a specific interface.
In case you want to work with clean and fresh data, you can use the command `clear ip access-list counters [access-list-number | access-list-name]` to clear the ACL hit count.

```Cisco IOS
Switch2#show ip access-list

```

https://www.cisco.com/c/en/us/support/docs/ip/access-lists/26448-ACLsamples.html
https://www.cisco.com/en/US/docs/ios-xml/ios/sec_data_acl/configuration/15-2mt/sec-dply-clr-ald-acl.html
