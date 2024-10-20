
By default, MSSQL uses ports `TCP/1433` and `UDP/1434`, and MySQL uses `TCP/3306`. However, when MSSQL operates in a "hidden" mode, it uses the `TCP/2433` port. We can use `Nmap`'s default scripts `-sC` option to enumerate database services on a target system:
