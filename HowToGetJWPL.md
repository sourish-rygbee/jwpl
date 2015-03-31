# How to get and use JWPL #

Depending on your requirements and your system setup, there are several ways to get and use JWPL.

## Use JWPL in your Java project ##

Using JWPL in your own Java project can be achieved in two ways.

If you use Maven as a build tool, the preferred way to integrate JWPL in you project is using the Maven dependencies. You don't have to download anything manually. See the [Developer Setup](DeveloperSetup.md) page for further information.

If you cannot or do not want to use Maven, can obtain "jars-with-dependencies" for each JWPL module from the Maven central website (see section below). These jars contain all necessary dependencies. In larger projects, you might run into incompatibility issues with external libraries. This can easier be avoided by using JWPL with Maven.

## Run JWPL components from the command line ##

If you want to run JWPL components directly from the command line (e.g. the DataMachine), you should use the fatjars that come with all third-party libraries directly built-in. These jars can be downloaded from [Maven Central](http://search.maven.org/#search|ga|1|tudarmstadt.ukp.wikipedia). Choose the JWPL component you want to use and download the respective "jar-with-dependencies.jar"