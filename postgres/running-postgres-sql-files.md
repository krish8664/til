# Running postgres sql scripts on command - ubuntu

To run any postgres command from the commandline/terminal make use of the `psql` command which is the postgres interactive terminal. 

- For this particular case where i wanted to run a set of sql that were in a file i made use of this `psql` command
  `psql -d <db> -a -f <file>`
  where the `-d` is followed by the database name, `-a` is to print all to terminal & `-f` is followed by the file i need to run


This is nothing that a `man psql` won't let you know or [click here](http://www.postgresql.org/docs/9.5/static/app-psql.html)