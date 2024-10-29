Facebook's [osquery](https://osquery.io/) is a proven, lightweight tool to gather process information from endpoints.

Osquery has a concept of "tables", similar to a database, that provide a SQL interface to structured data. It's important to understand the 2 different types of tables, normal and event, which operate very differently.




```shell-session
# osqueryi
Using a virtual database. Need help, type '.help'
osquery> .table 
=> appcompat_shims
  => arp_cache
  => atom_packages
  => authenticode
  => autoexec
  => azure_instance_metadata
  => azure_instance_tags
  => background_activities_moderator
  => bitlocker_info
  => carbon_black_info
  => carves
  => certificates
  => chassis_info
  => chocolatey_packages
```


		 sample sql commands for osquery
```
osquery>.help
osquery> .schema users
osquery>select gid, uid, description, username, directory from users;
osquery>select * from programs limit 1;
osquery>select name, version, install_location, install_date from programs limit 1;
osquery>select count(*) from programs;
osquery>select * from file;
osquery>select p.pid, p.name, p.path, u.username from processes p JOIN users u on u.uid=p.uid LIMIT 10;
```

