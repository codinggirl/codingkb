---
tags: [GitHub]
title: "Ajax Image Uploading (With Less\_Suck)"
created: '2019-09-04T08:40:44.460Z'
modified: '2019-09-04T08:40:55.248Z'
---

# Ajax Image Uploading (With Less Suck)

 [![Photo of Zurb](https://secure.gravatar.com/avatar/cdead06cfc53c7f95d7b4e13bbe4ba51?s=80&d=retro&r=pg)](https://css-tricks.com/author/zurb/)

Author

[Zurb](https://css-tricks.com/author/zurb/)

46 Comments

[Go to Comments](https://css-tricks.com/ajax-image-uploading/#comments)

Published

Jun 10, 2010

Updated

Apr 13, 2017

Easily manage projects with [monday.com](https://synd.co/2JziuUL)

This technology demo is courtesy of [ZURB](http://www.zurb.com/) and the post was co\-authored by ZURB and Chris.

**How do you upload images now?**

You select a file and click upload. Simple right? Except once you select your image you can no longer see what was selected. The name of the file is at the end of the input, and if the input is short, or the file path is deep, you're not going to see anything useful. You forget what you selected and have not idea what you're about to upload. *"Wait, did I upload a picture of my face or something less professional?"*

![](https://css-tricks.com/wp-content/csstricks-uploads/oldandbusted.png)

Let's make it better.

The key is having an image preview that gets shown before the user needs to commit to saving it. So we ditch the upload button in favor of a save button. Now when a file is selected via the file section input, some AJAX kicks in. The image is processed server side and a thumbnail is loaded onto the existing page.

![](https://css-tricks.com/wp-content/csstricks-uploads/newbetterimageupload.png)

Doesn't that feel so much better? We now have a visual representation (imagine that) of the image we selected.

This is particularly useful in larger forms when many fields will be submitted with a single action. It allows the user to review the form before pressing save and see what image (or images) they selected.

### [#](#article-header-id-0)How it's done

Curious how this is done? Here's the code.

We can use a standard HTML form. We'll include that as well as an image preview area.

```javascript
<div id="upload-area">
	<div id="preview">
		<img width="100px" height="100px" src="/images/icons/128px/zurb.png" id="thumb">
	</div>

	<form action="/playground/ajax_upload" id="newHotnessForm">
		<label>Upload a Picture of Yourself</label>
		<input type="file" size="20" id="imageUpload" class=" ">
		<button class="button" type="submit">Save</button>
	</form>
</div>
```

The action of the form should point somewhere the process the uploading of the image, regardless of the presence of JavaScript. You may also want to consider only appending the preview markup via JavaScript, as that won't work without JS.

You're going to need [jQuery](http://jquery.com) and the [AJAX Upload jQuery plugin](http://valums.com/ajax-upload/) by Andrew Valums. Link them up, making sure jQuery is loaded first. Note that the AJAX Upload plugin does require some configuration and server side stuff to work. Check out that link for details.

```markup
<script src="/js/jquery.min.js" type="text/javascript"></script>
<script src="/js/ajaxupload.js" type="text/javascript"></script>
```

Here is the JavaScript we're going to add in its entirety.

```javascript
$(document).ready(function(){

	var thumb = $('#thumb');

	new AjaxUpload('imageUpload', {
		action: $('#newHotnessForm').attr('action'),
		name: 'image',
		onSubmit: function(file, extension) {
			$('#preview').addClass('loading');
		},
		onComplete: function(file, response) {
			thumb.load(function(){
				$('#preview').removeClass('loading');
				thumb.unbind();
			});
			thumb.attr('src', response);
		}
	});
});
```

Here's that in plain English:

1.  Attach plugin behavior to the file input
2.  Specify the action URL, appropriately pulling it from the action URL in the markup
3.  When a file is selected, immediately add a class of "loading" to the preview area. This class name, through CSS, hides the image (e.g. .loading img { display: none; } and applies a background image ([a spinner](http://ajaxload.info/))
4.  Upon AJAX success, remove the loading class and set the src of the image to the response URL that the plugin returns.

### [#](#article-header-id-1)Preview Only

Note that the above code deals with the preview functionality only. The file section input still contains the proper file path. So when the user clicks the "Save" button, the form will still need to be processed and handled by whatever server side technology you have ready for that. So yes, you will be essentially uploading the image once (for the preview) and then uploading it again (to save it in a more permanent way).

When you are setting up/configuring the plugin, you'll set up a directory for a server side script to save uploaded images to. You might want to have that live at somewhere like /images/temp/. That way you know that anything uploaded to that directory is transient and can be cleaned out temporarily to keep the server clean. A empty\-that\-directory CRON job might be a good idea.

### [#](#article-header-id-2)Demo

A working demo is available on ZURB's site, which also includes more written information. Because of the server side stuff needing to happen, a download unfortunately isn't practical.

[View Demo](http://www.zurb.com/playground/ajax_upload)

Wondering about other awesome techniques with CSS3 and JavaScript? Check out the [ZURB Playground](http://www.zurb.com/playground/).

ZURB is a close\-knit team of interaction designers and strategists that help companies design better products & services. Since 1998 ZURB has helped over 75+ clients including: Facebook, eBay, NYSE, Yahoo, Zazzle, Playlist, Britney Spears, among others.

[Ajax Image Uploading (With Less Suck) | CSS-Tricks](https://css-tricks.com/ajax-image-uploading/)

