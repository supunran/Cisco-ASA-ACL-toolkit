# Cisco-ASA-ACL-toolkit
Utilities for parsing, analyzing and modifying Cisco ASA ACLs

Install netaddr:

```sh
pip install netaddr
```
## ipaclmatch.py 

ipaclmatch.py finds ACLs matching the given IP-addresses, including networks, subnetworks and supernets.

Usage:

```txt
 ipaclmatch.py --help
usage: ipaclmatch.py [-h] [--addr ADDR] [--acl ACL] [--sd SD] [--noany]

optional arguments:
  -h, --help   show this help message and exit
  --addr ADDR  Comma-separated list of addresses/netmasks
  --acl ACL    Cisco ASA ACL filename
  --sd SD      Where to search: source, dest or both
  --noany      Ignore 'any' in the ACLs
  --deny       Search 'deny' rules only
  --permit     Search 'permit' rules only
  --direct     Direct IP match only

```

Examples:

Save the access group using the following command in a file, for example ACL_name.acl:

```txt
sh access-list ACL-name
```

Search only the source addresses for 10.0.1.2/32 and 10.2.3.0/24 and networks these addresses contain or belong to (for example, 10/8, 10.2.3.128/25, 10.2.3.4/32, etc):

```txt
python ipaclmatch.py --addr 10.0.1.2,10.2.3.0/24 --acl ACL_name.acl --sd source
```

Search for 10.2.3.0/24 in both source and destination, but ignore "any":

```txt
python ipaclmatch.py --noany --addr 10.2.3.0/24 --acl ACL_name.acl 
```

Search deny rules with 10.2.3.0/24 in the destination and ignore "any" (in the dest):

```txt
python ipaclmatch.py --noany --addr 10.2.3.0/24 --acl ACL_name.acl --deny --sd dest
```

Search direct only matches (no subnets, supernets, etc, will be looked for) for 10.2.3.0/24 as the source address:

```txt
python ipaclmatch.py --noany --addr 10.2.3.0/24 --acl ACL_name.acl --sd dest --direct
```
