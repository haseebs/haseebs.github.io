---
layout: default
title:  "Retrieval of Visually Similar Garments"
date:   2015-12-13 02:54:25 +0500
categories: project
description: A Machine Learning model implemented in Tensorflow which
allows the retrieval of visually similar garment images.
---
# **Retrieval of Visually Similar Garments**
## **Overview**
**Semester:** 5th (Internship Project)

**Project Date:** July 2017

**Project Duration:** Ongoing

**Languages/Frameworks:** Tensorflow, Python, Bash

**Short Description:** A Machine Learning model implemented in Tensorflow which
allows the retrieval of visually similar garment images.

## **Problem Introduction**
Due to the popularity of smart phones, it is very easy for people to
take pictures of various things in their lives. These pictures can be
used as queries by the user to search for visually similar images on the
internet. Due to the popularity of online ecommerce websites, visually
similar product retrieval has become a very interesting research area.

One particular domain that benefits greatly from this type of search is
fashion. A user can simply take picture of his friend and upload it to
such search engine and fetch him/her all the visually similar fashion
images available in the store. So the problem of garment retrieval is,
given a query image containing some clothing item, retrieve a set of
visually similar clothing images present in the gallery.

This problem can be very tricky to solve and one of the main reasons for
that is because of the differences in images of the two different
domains. One domain contains the image of a clothing item mostly
captured from a user's smart phone devices, which may have poor quality,
occlusion, inadequate lightning, etc. While the other domain consists of
images that are mostly professionally captured and are thus of very good
quality. Due to this difference between the two domains, a common
approach nowadays is to apply deep-learning methodology and try to learn
a similarity measure that is invariant to domain-shift.
