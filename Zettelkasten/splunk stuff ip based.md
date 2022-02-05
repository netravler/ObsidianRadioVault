
```
| tstats values(All_Traffic.dest) values(All_Traffic.dest_port) from datamodel=Network_Traffic where sourcetype=cisco:asa NOT All_Traffic.dest IN `jha_ip_addresses` (All_Traffic.action="allowed") (All_Traffic.src IN (10.203.165.0/24) OR All_Traffic.src_ip IN (10.203.165.0/24) OR All_Traffic.src_mac IN (10.203.165.0/24) OR All_Traffic.src_translated_ip IN (10.203.165.0/24)) (All_Traffic.transport=*) by All_Traffic.src
| `drop_dm_object_name("All_Traffic")`
```
