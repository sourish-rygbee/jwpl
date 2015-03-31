[Back to the JWPL Core overview page](JWPL_Core.md)

MySQL 4.x supports only 4GB MyISAM tables with the default settings.
Unfortunately, the English Wikipedia is already much larger than this. Given the growth rates, many Wikipedias will reach that size sooner than later.

The solutions on this page where provided by Christian Pietsch.

## Solution 1 ##

You may need to have root access, depending on you server's configuration.

If you are using a MySQL version before 5.0.6 and at least 4.1.2, do prior to import on the mysql command line:

```
SET GLOBAL max_allowed_packet=1000000000;

SET GLOBAL myisam_data_pointer_size=5;
```

## Solution 2 ##

Adapted from jeremy.zawodny.com/blog/archives/000796.html and untested.

```
gunzip < DUMPFILE.sql.gz | \

sed 's/^) ENGINE=MyISAM DEFA/) ENGINE=MyISAM MAX_ROWS=200000000000 AVG_ROW_LENGTH=50 DEFA/g' | \

mysql -uUSER -p --default-character-set=utf8 --max_allowed_packet=1000000000 DB_NAME
```