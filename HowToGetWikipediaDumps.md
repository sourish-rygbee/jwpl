# Wikipedia Dump Repository #
You can find the Wikipedia data dumps on http://dumps.wikimedia.org/

Each language version has it's own subdirectory, e.g. for English this is http://dumps.wikimedia.org/enwiki

# Download Tool for Revision Dumps #
The Wikipedia revision is split into many files. Downloading them manually can be cumbersome.

You can find a download tool for Wikipedia dumps at http://github.com/babilen/wp-download/

It can be used for bulk downloads all dump files for any language.
It also supports resuming aborted downloads.

(You don't need the tool to download the three dump files for JWPL Core. It only makes sense for the revision dumps.)

```
$ wp-download -v --resume /path/to/wikipedia/dumps
Read configuration from: '/home/foobar/.wpdownloadrc'
Set timeout to 30s
Processing language: sw
Creating directory: /path/to/wikipedia/dumps/sw/20090821
Latest dump for (sw) is from Friday 21 August 2009
Skip: swwiki-20090821-redirect.sql.gz
Skip: swwiki-20090821-category.sql.gz
Resume: swwiki-20090821-pages-articles.xml.bz2
swwiki-20090821-pages-articles.xml.bz2 [****] 100% Time: 00:00:00   3.19 M/s
```

For further instructions/support, please refer to the website of the tool.