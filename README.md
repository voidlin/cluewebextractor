ClueWeb Extractor
================
cluewebextractor takes warc files from Clueweb, or wikiExtractions from Wikipedia
and extracts sentences from each warc record's payload.

On error, cluewebextractor prefers to skip over the smallest possible amount of
data rather than crash. The amount skipped can be either a sentence, a warc
record, or an entire warc file (which should be relatively rare).

For each sentence found, outputs a tab-separated line with fields:

1. warc trec-id
2. url
3. sentence number
4. sentence

Quick Start:
------------

To make the jar file, run:

    mvn compile scala:compile assembly:single

Usage:
------

    java -jar \<jarfile\> \<input\> --output-dir \<output-dir\> --input-type \<warc/wiki\>

Inputs can be either a single file or a directory. If a directory,
cluewebextractor will find all the `.warc` and `.gz` files inside of that
directory and extract content from them.

`--output-dir` is an optional switch that specifies an output directory for the
extracted content. If not used, cluewebextractor will either: not use a
directory, if input is a single file, or; use the name of the input directory
as the output directory, if input is a directory.

 --input-type is an optional switch that specifies the type of input file 
cluewebextractor is extracting. It can be either wiki or warc. If not specified,
it assumes that it is extracting a warc file.

The extractor will print out to an output file for each input file.

Extract Wikipedia:
------------------

To get the articles from Wikipedia, the first step is to download pages-articles.xml.bz2
file from Wikipedia Database Page. 

Instructions can be found at this website:
http://medialab.di.unipi.it/wiki/Wikipedia_Extractor

To filter out the Wiki Mark-Up, run WikiExtractor.py, located in rv-n15,
/scratch/usr/wy1024/wiki_page_articles/wikiExtractor

Run:

bzcat itwiki-latest-pages-articles.xml.bz2 | WikiExtractor.py -cb 250K -o extracted

Then combine the whole extracted text into a single file run:

find extracted -name '*bz2' -exec bunzip2 -c {} \; > text.xml
