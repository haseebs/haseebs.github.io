---
layout: default
title:  "WikiHunt: Simple Wikipedia Search Engine"
date:   2015-12-13 02:54:25 +0500
categories: project
description: WikiHunt is a scalable search engine. It involved generation of inverted index, calculating IR scores using different weights and then sorting the results.

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

2. We iterate over each page, and in each page, we iterate over all the
   words. Each word's hit type is determined an stored as the respective
   hit in array. Each hit is encoded into 32-bit integer using bitwise
   operators. A hit contains information about the capitalization,
   importance and the position of the word.
   
3. The array is then parsed and converted into a MySQL file which contains
   multiple MySQL statements, each containing 5000 row entries. This was
   done instead of using the mysql library because generation of forward
   index by executing the generated mySQL file resulted in creation of
   forward index 80 times faster than the library method. This is believed
   to be because multiple hits are appended to be inserted as one query as
   compared to a separate query for each new hit.
   
### **Generation of Inverted Index**
For the generation of inverted index, we follow the following steps:  
1. Get the max Word ID from the database.
2. Add index on Word ID in the forward index to ensure faster retrieval
3. For each Word ID, retrieve the hits containing it.
4. Generate the mySQL file for inverted index
5. Drop index from the inverted index and the forward index before executing
   the mySQL file.
   
### **Search Process**
When a query arrives, it is first determined whether the query is a single word
or a multi-word query, then it is processed accordingly:
#### **Single Word Search**
1. We retrieve wordID of the word
2. Hit information is extracted for that word
3. IR score is calculated by dot product of Hit vector with Typeweight vector.
4. Top 100 results are returned when sorted by IR scores.

#### **Multi-Word Search**
1. We retrieve wordID of the each word
2. Hit information is extracted for each word
3. Only those documents are kept which have all the query words.
4. Proximity vector (contains distance between the query words)
and Hit vectors dot products with Proximity-weight vector
and Typeweight vector are calculated respectively.
3. IR score is calculated from the dot product of resulting proximity and type
weights.
4. Top 100 results are returned when sorted by IR scores.