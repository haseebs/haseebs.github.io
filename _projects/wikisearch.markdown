---
layout: default
title:  "WikiHunt: Simple Wikipedia Search Engine"
date:   2015-12-13 02:54:25 +0500
categories: project
description: WikiHunt is a scalable search engine. It involved
generation of inverted index, calculating IR scores using different
weights and then sorting the results.

---
# **WikiHunt: Simple Wikipedia Search Engine**
## **Overview**
**Semester:** 3rd

**Project Date:** December 2016

**Project Duration:** 1 week

**Languages/Frameworks:** Ruby, MySQL, Sinatra, HTML, CSS

**Github Link:** [Link](https://www.github.com/haseebs/search-engine-ruby)

**Short Description:** WikiHunt is a scalable search engine. It involved
generation of inverted index, calculating IR scores using different
weights and then sorting the results. Various parts of the
implementation were profiled and optimized for performance.

## **Description**
WikiHunt is made as a search engine for [Simple
Wikipedia](https://simple.wikipedia.org/wiki/Main_Page). The entirety of
this wikipedia was downloaded from the data dumps in XML format, which
was then parsed using Nokogiri gem.

### **Parsing**
The data was obtained in form of a single XML file in which each page is
separated by <page> tag. This file is read by repositoryGenerator.rb
which uses Nokogiri gem for parsing the XML. The content of each page
is separated and stored in files and named as
**page_id**-**page_title**-**number_of_words**.

### **Generation of Lexicon**
Afterwards, a Lexicon is generated as a table in MySQL database. We
iterate through each page in the repository and using regex, we:
1. Split the content string based on any whitespace or non-word character
2. Select the elements from the resulting array that consist of word
characters.
3. These are pushed into Lexicon with Insert Ignore, which automatically
   rejects duplicate word entries, and each word is mapped to a unique
   word ID.

### **Generation of Forward Index**
For the generation of forward index, we follow the following steps:
1. Load the entire Lexicon from the database into RAM. It is usually
   small so it can be loaded entirely into the RAM easily. This is done
   because otherwise we would have to send queries for each word
   separately, and if we do that, it would take us 10-30 weeks longer to
   complete the generation of forward index.

2. In progress ...
