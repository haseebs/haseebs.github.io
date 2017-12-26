---
layout: default
title:  "Photron Image Translator"
date:   2015-12-13 02:54:25 +0500
categories: project
description: Windows 10 Universal application (Desktop + phone + Xbox + Hololens) that takes an image as input, extracts the text from it with OCR and translates the text to the desired language. It has more than 25,000 downloads to date.

---
# **Photron Image Translator**
## **Overview**
**Semester:** 1st

**Project Date:** December 2015

**Team Members:** None

**Project Duration:** 2 weeks

**Languages/Frameworks:** C#, XAML

**Downloads:** ~25,000

**Download Link:** [Link](https://www.microsoft.com/store/apps/9nblggh58rz3)

**Platforms Supported:** Windows 10, Windows Phone 10, Xbox, Hololens

**Short Description:** Photron Image Translator is a Windows 10 Universal application that takes an image as input, extracts the text from it with OCR and translates the text to the desired language. This application was built when windows 10 and windows phone 10 were still in beta (Insider preview), so the userbase was pretty small.

## **Features**
There are four major features of this application.
1. OCR capability

    There is an OCR capability present in the application which allows the user to extract text from one of the 25 available languages offline.

2. Text-to-Text Translation Capability

    Microsoft's Text Translation API was used which provides online machine translation capability between 50 languages. The free tier allows translation for upto 2 million characters per month. Other good alternatives are Yandex Translate and Google Translate but those two do not have free tiers anymore.

3. Text-to-Speech Capability

    Offline API was used for text-to-speech conversion. It uses the voice packs installed in the OS to read the text. There is an option provided in the application allowing the user to choose the desired voice.
    
4. Sharing Capability

    System's API were used allowing the user to share the extracted text at any stage while he is using the application.

## **Design Philosophy**
One of the most challenging and fun part of designing this application was its design. Half of the total time was spent on planning  and implementing the design. I wanted the design to be obvious, easy to use and minimalistic both aesthetically and functionally.

### **Logo Design**
![Logo](/assets/media/photron/photron_logo.jpg)

The logo follows our design philosophy. The outer triangles portray a camera shutter and the inner lines portray text. They perfectly describes what this application can do.

### **UI Design**
When someone opens up the application, he should know exactly what he wants to do and not be confused by other elements. So, after days of brainstorming, I drew wireframes on a paper and came up with something that satisfied the design philosophy.

The application has 3 main functions:
1. Extract the text from image
2. Translate any piece of text
3. Read aloud any piece of text

So there are 3 different ways a user may use my app:

1. Choose image source -> Select image -> Extract text
2. Enter text / Use extracted text -> Choose language -> Translate
3. Enter text / Use extracted text  -> Choose voice -> Listen to text

This shows that when someone opens the application, there are 2 ways he may proceed. He may choose to do one of:

1. Enter Text
2. Choose an Image source

So if he is presented with just 2 buttons at the start, all use cases are covered and the user will not be confused.

![Main Page](/assets/media/photron/main_screen.jpg)

So there are 3 pages : Sources, Extracted text and Translation. The 3 functions of the application are achieved quickly and easily as:

1. **Extract text:** Press Image -> Choose source -> Extracted Text tab becomes active and shows the extracted text. To translate this text, simply press translate below it.
2. **Translate Text:** Press Text -> Enter Text -> Choose language -> Translation tab becomes active and shows translation.
3. **Read Text:** Press Speak button present below any text-> Choose voice -> Press Play

Since the user may choose to translate from any page, the language bar was made persistent between the tabs.

If the user Swipes to other tab without providing any input, he is provided with a helpful message telling him what he is doing and what he should do.

![Input Missing](/assets/media/photron/input_missing.jpg)

## **Video Demo**
<iframe width="560" height="315" src="https://www.youtube.com/embed/_GtYWHLkSjw?rel=0&amp;controls=0&amp;showinfo=0" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>
