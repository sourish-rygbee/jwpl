[Back to the JWPL Core overview page](JWPL_Core.md)

## Getting Started ##
  1. Create the JWPL database(s) using the DataMachine or TimeMachine.
    * Use the DataMachine, if you just need a single database from a certain date.
    * Use the TimeMachine (part of the Revision Toolkit) to create multiple Wikipedia databases corresponding to past states of Wikipedia
  1. You can now access the JWPL databases with the JWPL API.
    * You have several possibilities how to use the API
      * Ceck out the API from SVN and build it using Maven
      * Add it as a dependency to your existing Maven project as described in the DeveloperSetup
      * Use one of the prebuilt jars in the [download section](http://code.google.com/p/jwpl/downloads/list).
    * Follow the examples in the [tutorial](JwplTutorial.md). The tutorials are also included in the source code and the archives.
  1. If you encounter any problems, please check the [FAQ](JWPL_FAQ.md) and the [JWPL Mailing List](http://groups.google.com/group/jwpl).

**Note:** If you need access to the revision history of the pages in your JWPL database, you might want to check out the RevisionMachine, which adds the revision data to your existing database and provides an additional API (RevisionAPI) to access the data.