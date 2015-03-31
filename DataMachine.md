[Back to overview page.](JWPLDocumentation.md)

# Usage #

  1. Learn about the different ways to [get JWPL](HowToGetJWPL.md) and choose the one that is right for you! (You might want to get fatjars with built-in dependencies instead of the download package on Google Code)
  1. [Download the Wikipedia data](HowToGetWikipediaDumps.md) from the Wikimedia Download Site
    * You need 3 files:
      * `[LANGCODE]wiki-[DATE]-pages-articles.xml.bz2` **OR** `[LANGCODE]wiki-[DATE]-pages-meta-current.xml.bz2`
      * `[LANGCODE]wiki-[DATE]-pagelinks.sql.gz`
      * `[LANGCODE]wiki-[DATE]-categorylinks.sql.gz`
    * Note: If you want to add discussion pages to the database, use `[LANGCODE]wiki-[DATE]-pages-meta-current.xml.bz2`, otherwise `[LANGCODE]wiki-[DATE]-pages-articles.xml.bz2` suffices.
  1. Run the transformation:
    * `java -jar JWPLDataMachine.jar [LANGUAGE] [MAIN_CATEGORY_NAME] [DISAMBIGUATION_CATEGORY_NAME] [SOURCE_DIRECTORY]` or `de.tudarmstadt.ukp.wikipedia.datamachine.domain.JWPLDataMachine [LANGUAGE] [MAIN_CATEGORY_NAME] [DISAMBIGUATION_CATEGORY_NAME] [SOURCE_DIRECTORY]`
      * LANGUAGE - a language string matching one the [JWPL\_Languages](JWPL_Languages.md).
      * MAIN\_CATEGORY\_NAME - the name of the main (top) category of the Wikipedia category hierarchy
      * DISAMBIGUATION\_CATEGORY\_NAME - the name of the category that contains the disambiguation categories
      * SOURCE\_DIRECTORY - the path to the directory containing the source files
    * **Attention:** For large dumps you need to increase the amount of memory assigned to the JVM using e.g. the "-Xmx2g" flag to assign 2GB of memory. For the recent English dump you will need at least 4GB.
    * **Attention:** If your system uses a different file encoding you might need to add the `-Dfile.encoding=utf8` flag.
  1. Patience :) This is going to take a while ...
    * While the DataMachine is running, it creates three temporary .bin files. These are only needed during processing time and can be deleted once all files in the output directory have been produced.
  1. Eventually, you should get 11 txt files in an "output" subfolder
  1. Create a database with the necessary tables
    * If you are using MySQL 4.x or previous - please see [JWPL\_MySQL4](JWPL_MySQL4.md).
    * Make sure the database encoding is set to UTF8.
    * Create a database
      * `mysqladmin -u[USER] -p create [DB_NAME] DEFAULT CHARACTER SET utf8;`
      * or when on the mysql shell: `CREATE DATABASE [DB_NAME] DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;`
    * Create all necessary tables using [jwpl\_tables.sql](http://code.google.com/p/jwpl/source/browse/trunk/de.tudarmstadt.ukp.wikipedia.wikimachine/jwpl_tables.sql)
  1. Import the data files into the database.
    * `mysqlimport -uUSER -p --local --default-character-set=utf8 {database_name} ````pwd`````/*.txt``
  1. Now you are ready to use the database with the JWPL Core API. (also see [JWPLCore:GettingStarted](http://code.google.com/p/jwpl/wiki/JWPLCore_GettingStarted)) When first connecting to a newly imported database, indexes are created. This takes some time (up to 30 minutes), depending on the server and the size of your Wikipedia. Subsequent connects won't have this delay.

## Example Transformation Commands ##
(Note: increase heap space for large Wikipedia versions with the -Xmx flag)

  * `java -Xmx4g -jar JWPLDataMachine.jar english Contents Disambiguation_pages ~/enwiki/20081013`
  * `java -jar JWPLDataMachine.jar simple_english Contents Disambiguation ./`
  * `java -jar JWPLDataMachine.jar spanish Índice_de_categorías Wikipedia:Desambiguación ~/eswiki/20080416/`
  * `java -Xmx2g -jar JWPLDataMachine.jar german !Hauptkategorie Begriffsklärung ~/dewiki/20080422/`
  * `java -jar JWPLDataMachine.jar dutch Alles Doorverwijspagina ~/nlwiki/200810408`

Mind that the names of the main category or the category marking disambiguation pages may change over time. E.g. the English category for disambiguation pages was called "Disambiguation" for a long time, while now it is "Disambiguation\_pages".

## Discussion Pages ##

Discussion pages can only be included if the source file contains these pages (see above).<br />
All discussion pages will be marked with the prefix "Discussion:" in the page title (for all wikipedia language versions).