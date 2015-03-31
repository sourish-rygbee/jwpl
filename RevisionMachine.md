[Back to overview page.](WikipediaRevisionToolkit.md)

# Creating the Revision Database #

**This description is still under construction - it only contains the absolutely necessary basics for getting started. It is based on RevisionMachine 0.9.0**

  1. Learn about the different ways to [get JWPL](HowToGetJWPL.md) and choose the one that is right for you! (You might want to get fatjars with built-in dependencies instead of the download package on Google Code)
    1. [Download the Revision Dump](HowToGetWikipediaDumps.md) from the Wikimedia Download Site.
  1. Run _de.tudarmstadt.ukp.wikipedia.revisionmachine.difftool.config.gui.ConfigGUI_. This is a SWING application which preloads a standard configuration for the RevisionMachine upon startup. You have to at least adapt the input and output paths. Save the configuration somewhere to your disk. Note that 7zip input is only available if you have configured the 7z command line tool in the "Externals" tab. bz2 works without any additional configuration. You also have to decide whether you want to produce SQL dumps or CSV data files. The latter option imports faster.
  1. Run _de.tudarmstadt.ukp.wikipedia.revisionmachine.difftool.DiffTool_. As a single parameter, the DiffTool takes the path to the configuration file you created in the first step. Give the process enough heap space - at least 4GB for the English Wikipedia. This process takes quite some time. If you want to process the whole English Wikipedia in one single process, it should take 7 to 10 days. As the Wikipedia XML dump files are usually split into several independent parts, you can create several config files (see step 1) - each configured to read 1 or more xml dump parts, Then it is possible to run several instances of the DiffTool in parallel (each with a different config file). This reduces the processing time to 1 to 3 days or less
  1. Each DiffTool instance produces one or more sql, sql.bz2 or csv files (depending on your configuration).
    * These files have to be imported either into an empty db or into a db containing the [JWPL data](http://code.google.com/p/jwpl/wiki/DataMachine) generated from the same Wikipedia XML dump you also created the revision dump with. When creating a new db, make sure the database encoding is set to UTF8:
      * `mysqladmin -u[USER] -p create [DB_NAME] DEFAULT CHARACTER SET utf8;`
      * or when on the mysql shell: `CREATE DATABASE [DB_NAME] DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;`
    * SQL files can be directly imported into the database. However, the import is rather slow.
    * A much faster option is importing the data from CSV files. The data files can be imported using _mysqlimport_ or with the _LOAD DATA INFILE_ command on the MySQL console. (Further [Instructions for CSV Import](InstructionsCSVImport.md))
  1. When all imports have finished, you need to perform one last step. You need to run _de.tudarmstadt.ukp.wikipedia.revisionmachine.index.IndexGenerator_. As the only parameter, the IndexGenerator takes the path to a configuration file, which contains the connection data to the database with the freshly imported revisions. A sample can be seen found [here](http://code.google.com/p/jwpl/source/browse/tags/de.tudarmstadt.ukp.wikipedia-0.9.1/de.tudarmstadt.ukp.wikipedia.revisionmachine/src/main/resources/configSamples/indexGenerator_config_sample). Again, you can choose between SQL or CSV output which you have to import into the database containing the revisions. (Further [Instructions for CSV Import](InstructionsCSVImport.md))
  1. Now everything is done and the revisions can be accessed with the RevisionAPI.

# More Information #
For a description of the concept behind the RevisionMachine, refer to [the following paper](http://www.ukp.tu-darmstadt.de/publications/details/?no_cache=1,0,&pub_id=TUD-CS-2011-0110) or have a look at [this poster](http://jwpl.googlecode.com/svn/wiki/doc/ACL_2011_Poster.pdf).