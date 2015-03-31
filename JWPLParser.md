[Back to the JWPL Core overview page](JWPL_Core.md)

**NOTE: As of JWPL 0.9.2, the JWPL Parser will not be supported any more. It has been moved from the API module to a PARSER Module.**

The JWPL parser analyzes the structure of a text with MediaWiki markup and represents it as a Java object. This allows for structured access to the contents of e.g. Wikipedia or Wiktionary. There is no standalone release of the parser, as it is part of the JWPL Wikipedia API release. However, it can be used perfectly without accessing Wikipedia with JWPL.

## Code Example ##

```
// get a ParsedPage object
MediaWikiParserFactory pf = new MediaWikiParserFactory();
MediaWikiParser parser = pf.createParser();
ParsedPage pp = parser.parse(MEDIA_WIKI_FORMATTED_TEXT);
		
// only the links to other Wikipedia language editions
for (Link language : pp.getLanguages()) {
    System.out.println(language.getTarget());
}
    
//get the internal links of each section
for (Section section : pp.getSections()){
    System.out.println("Section: " + section.getTitle());

    for (Link link : section.getLinks(Link.type.INTERNAL)) {
        System.out.println("  " + link.getTarget());
    }
}
```

## Configuring the Parser ##

This will configure the parser to drop any templates.

```
MediaWikiParserFactory pf = new MediaWikiParserFactory();
pf.setTemplateParserClass( FlushTemplates.class );

MediaWikiParser parser = pf.createParser();
ParsedPage pp = parser.parse(SOME_WIKI_MARKUP);
```