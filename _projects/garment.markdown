---
layout: default
title:  "Retrieval of Visually Similar Garments"
date:   2015-12-13 02:54:25 +0500
categories: project
description: A Machine Learning model implemented in Tensorflow which allows the retrieval of visually similar garment images.

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

## **Demo**
Upload any jpeg, jpg or png image of a clothing item or press the get
random image button and the model will retrieve visually similar images
from among those currently stored in the gallery. The uploaded image will
be resized to 160x160 using a box filter with equal weights before inference.
The inference is performed by the model hosted on Google Cloud (CPU) and the
results are obtained from the gallery of ~60k images I stored on AWS S3.

**Note: Demo is currently offline due to hosting costs.**

<style>
* {
    box-sizing: border-box;
}

.row {
    display: -ms-flexbox; /* IE10 */
    display: flex;
    -ms-flex-wrap: wrap; /* IE10 */
    flex-wrap: wrap;
    padding: 0 4px;
}

.column {
    -ms-flex: 25%; /* IE10 */
    flex: 25%;
    max-width: 25%;
    padding: 0 4px;
}

.column img {
    margin-top: 8px;
    vertical-align: middle;
}

@media (max-width: 800px) {
    .column {
        -ms-flex: 50%;
        flex: 50%;
        max-width: 50%;
    }
}


@media (max-width: 600px) {
    .column {
        -ms-flex: 100%;
        flex: 100%;
        max-width: 100%;
    }
}

.btns {
    width: 0.1px;
    height: 0.1px;
    opacity: 0;
    overflow: hidden;
    position: absolute;
    z-index: -1;
}

.btns + label {
    width: 260px;
    max-width: 80% !important;
    font-size: 1.25rem;
    /* 20px */
    font-weight: 700;
    text-overflow: ellipsis;
    white-space: nowrap;
    cursor: pointer;
    display: inline-block;
    overflow: hidden;
    padding: 0.625rem 1.25rem;
    /* 10px 20px */
}

.btns:focus + label,
.btns.has-focus + label{
    outline: 1px dotted #000;
    outline: -webkit-focus-ring-color auto 5px;
}

#file1+ label svg {
    width: 1em;
    height: 1em;
    vertical-align: middle;
    fill: currentColor;
    margin-top: -0.25em;
    /* 4px */
    margin-right: 0.25em;
    /* 4px */
}


.btns + label {
    color: #f1e5e6;
    background-color: #d3394c;
}

.btns:focus + label,
.btns.has-focus + label,
.btns + label:hover {
    background-color: #722040;
}


.btns + label {
	cursor: pointer; /* "hand" cursor */
}

.btns:focus + label {
	outline: 1px dotted #000;
	outline: -webkit-focus-ring-color auto 5px;
}

.center {
    margin: auto;
}

</style>

<form enctype="multipart/form-data" style="text-align: center;">
    <input class="btns" type="file" name="file" id="file1"/>
    <label for="file1"><svg xmlns="http://www.w3.org/2000/svg" width="20" height="17" viewBox="0 0 20 17"><path d="M10 0l-5.2 4.9h3.3v5.1h3.8v-5.1h3.3l-5.2-4.9zm9.3 11.5l-3.2-2.1h-2l3.4 2.6h-3.5c-.1 0-.2.1-.2.1l-.8 2.3h-6l-.8-2.2c-.1-.1-.1-.2-.2-.2h-3.6l3.4-2.6h-2l-3.2 2.1c-.4.3-.7 1-.6 1.5l.6 3.1c.1.5.7.9 1.2.9h16.3c.6 0 1.1-.4 1.3-.9l.6-3.1c.1-.5-.2-1.2-.7-1.5z"/></svg> <span>Upload Image file</span></label>
</form>

<div style="text-align:center;">
    <button class="btns" id="random"></button>
    <label for="random"><span>Get Random Image</span></label>
</div>
<br>

<div style="text-align:center;">
    <img id="src" src=""/>
</div>
          
<br>
<div id="results" class="row">
    <div class="column">
        <img src=""/>
        <img src=""/>
        <img src=""/>
        <img src=""/>
    </div>
    <div class="column">
        <img src=""/>
        <img src=""/>
        <img src=""/>
        <img src=""/>
    </div>
    <div class="column">
        <img src=""/>
        <img src=""/>
        <img src=""/>
        <img src=""/>
    </div>
    <div class="column">
        <img src=""/>
        <img src=""/>
        <img src=""/>
        <img src=""/>
    </div>
</div>
<script>
 $("#file1").on("change", function() {
    formdata = new FormData();
    var file = this.files[0];
    if (formdata) {
        formdata.append("file", file);
        jQuery.getJSON({
            url: "https://garment-retrieval.appspot.com/",
            type: "POST",
            data: formdata,
            processData: false,
            contentType: false,
            success: function(response) {
                $('#src').attr("src", "");
                $('#results img').each(function(index) {
                    $(this).attr("src", "");
                    $(this).attr("src", response[index]);
                });
            }
        });
    }
});
$("#random").on("click", function() {
    jQuery.getJSON({
        url: "https://garment-retrieval.appspot.com/random",
        type: "GET",
        processData: false,
        contentType: false,
        success: function(response) {
            $('#src').attr("src", response[0]);
            $('#results img').each(function(index) {
                $(this).attr("src", "");
                $(this).attr("src", response[index+1]);
            });
        }
    });
});
    
/*
	Script below by Osvaldas Valutis
	Available for use under the MIT License
*/
'use strict';

;( function ( document, window, index )
{
	var inputs = document.querySelectorAll( '#file1' );
	Array.prototype.forEach.call( inputs, function( input )
	{
		var label	 = input.nextElementSibling,
			labelVal = label.innerHTML;

		input.addEventListener( 'change', function( e )
		{
			var fileName = '';
			if( this.files && this.files.length > 1 )
				fileName = ( this.getAttribute( 'data-multiple-caption' ) || '' ).replace( '{count}', this.files.length );
			else
				fileName = e.target.value.split( '\\' ).pop();

			if( fileName )
				label.querySelector( 'span' ).innerHTML = fileName;
			else
				label.innerHTML = labelVal;
		});

		// Firefox bug fix
		input.addEventListener( 'focus', function(){ input.classList.add( 'has-focus' ); });
		input.addEventListener( 'blur', function(){ input.classList.remove( 'has-focus' ); });
	});
}( document, window, 0 ));
</script>
