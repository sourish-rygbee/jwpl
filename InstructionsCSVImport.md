# Introduction #

Instead of creating and import SQL files, the RevisionMachine is able to produce CSV data files. The following instructions will show you how to import them


# Revision Data #

To import the csv-output of the DiffTool, you have to create a table from the following schema and then import the csv file into the same database:

```
CREATE TABLE IF NOT EXISTS revisions (
PrimaryKey INT UNSIGNED NOT NULL AUTO_INCREMENT, 
FullRevisionID INTEGER UNSIGNED NOT NULL, 
RevisionCounter INTEGER UNSIGNED NOT NULL, 
RevisionID INTEGER UNSIGNED NOT NULL, 
ArticleID INTEGER UNSIGNED NOT NULL, 
Timestamp BIGINT NOT NULL, 
Revision MEDIUMTEXT NOT NULL, 
Comment MEDIUMTEXT, 
Minor TINYINT NOT NULL, 
ContributorName TEXT NOT NULL, 
ContributorId INTEGER UNSIGNED, 
ContributorIsRegistered TINYINT NOT NULL, 
PRIMARY KEY(PrimaryKey)
) ENGINE= MyISAM DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
```

The following command can be executed on the MySQL console. Alternatively, you can use the mysqlimport tool. Please refer to the manpage of the tool for instruction.

```
load data local infile '/path/to/file.csv' into table revisions
fields terminated by ','
optionally enclosed by '"'
escaped by '\\'
lines terminated by ';'
(PrimaryKey,FullRevisionID,RevisionCounter,RevisionID,ArticleID,Timestamp,Revision,Comment,Minor,ContributorName,ContributorId,ContributorIsRegistered);
SHOW WARNINGS;
```

After the data has been imported, you need to create all necessary indexes. This might take a couple of hours.

```
CREATE UNIQUE INDEX revisionIdx ON revisions(RevisionID);
CREATE INDEX articleIdx ON revisions(ArticleID);
```

# Revision Index #
Similar to the DiffTool, the IndexGenerator can also produce CSV files.

Create tables:

```
CREATE TABLE index_articleID_rc_ts (
ArticleID INTEGER UNSIGNED NOT NULL, 
FullRevisionPKs MEDIUMTEXT NOT NULL, 
RevisionCounter MEDIUMTEXT NOT NULL, 
FirstAppearance BIGINT NOT NULL, 
LastAppearance BIGINT NOT NULL
) ENGINE= MyISAM DEFAULT CHARSET utf8 COLLATE utf8_general_ci;

CREATE TABLE index_revisionID (
RevisionID INTEGER UNSIGNED NOT NULL, 
RevisionPK INTEGER UNSIGNED NOT NULL, 
FullRevisionPK INTEGER UNSIGNED NOT NULL
) ENGINE= MyISAM DEFAULT CHARSET utf8 COLLATE utf8_general_ci;

CREATE TABLE index_chronological (
ArticleID INTEGER UNSIGNED NOT NULL, 
Mapping MEDIUMTEXT NOT NULL, 
ReverseMapping MEDIUMTEXT NOT NULL
) ENGINE= MyISAM DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
```

Load data:

```
load data local infile '/path/to/file/articleIndex.csv' into table index_articleID_rc_ts
fields terminated by ','
optionally enclosed by '"'
lines terminated by '\n'
(ArticleID,FullRevisionPKs,RevisionCounter,FirstAppearance,LastAppearance);

load data local infile '/path/to/file/revisionIndex.csv' into table index_revisionID
fields terminated by ','
optionally enclosed by '"'
lines terminated by '\n'
(RevisionID,RevisionPK,FullRevisionPK);

load data local infile '/path/to/file/chronoIndex.csv' into table index_chronological
fields terminated by ','
optionally enclosed by '"'
lines terminated by '\n'
(ArticleID,Mapping,ReverseMapping);
```

Create keys/indexes:

```
ALTER TABLE index_revisionID ADD PRIMARY KEY (RevisionID);
ALTER TABLE index_articleID_rc_ts ADD PRIMARY KEY (ArticleID);
ALTER TABLE index_chronological ADD PRIMARY KEY (ArticleID);
```