# The server requested password-based authentication, but no password was provided

I was getting this issue working with postgres recently, the db has no password set and the table owner was set to my user. It had me wondering what was causing this issue, so it happens that the tcp/ip socket connection for postgres expects a password based on the config file. so apparently it was set to md5 even for loclhost connection, which would promt password even if no password is set. so changing that to trust solved that issue.

- open to `/etc/postgresql/*/main/pg_hba.confg`
- chage *from* ```host    all         all         127.0.0.1/32          md5```
- *to* ```host    all         all         127.0.0.1/32          trust```
- Then reload the config by `sudo service postgresql reload`