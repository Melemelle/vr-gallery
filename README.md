# vr-gallery
Gallery prefab test

# Remote Gallery Slideshow for VRChat

## Overview

Remote Gallery Slideshow is a VRChat-compatible image gallery system that displays images hosted online.

Images can be changed without rebuilding or re-uploading your world. Simply update your GitHub-hosted image files and manifest file.

Features include:

* Remote image loading
* Automatic slideshow
* Previous / Next buttons
* Optional captions
* Random starting image
* Per-slide display durations
* Image caching
* Loading and error placeholders
* Quest compatible

---

# How It Works

The gallery uses two things:

## 1. Image URL List (inside Unity)

These are the approved image URLs that the world is allowed to display.

Example:

Element 0
https://yourname.github.io/gallery/image_01.png

Element 1
https://yourname.github.io/gallery/image_02.png

Element 2
https://yourname.github.io/gallery/image_03.png

## 2. Manifest File (hosted online)

The manifest controls:

* image order
* captions
* display duration

Example:

0|First Image|10
1|Second Image|15
2|Third Image|10

Format:

ImageIndex|Caption|Seconds

---

# Initial Setup

## Step 1 - Create GitHub Pages

Create a GitHub repository.

Example:

vr-gallery

Enable GitHub Pages.

Example URL:

https://yourname.github.io/vr-gallery/

---

## Step 2 - Upload Images

Upload images to the repository.

Example:

image_01.png
image_02.png
image_03.png

Direct image URLs become:

https://yourname.github.io/vr-gallery/image_01.png

https://yourname.github.io/vr-gallery/image_02.png

https://yourname.github.io/vr-gallery/image_03.png

---

## Step 3 - Create Manifest

Create:

manifest.txt

Example:

0|Welcome|10
1|Artwork|15
2|Photography|10

Upload it to GitHub.

Manifest URL:

https://yourname.github.io/vr-gallery/manifest.txt

---

# Unity Setup

## Gallery Object

Assign:

Manifest URL

Example:

https://yourname.github.io/vr-gallery/manifest.txt

---

## Image URLs

Set the Image URL Array Size.

Recommended:

50

Fill only the slots you currently use.

Example:

Element 0
https://yourname.github.io/vr-gallery/image_01.png

Element 1
https://yourname.github.io/vr-gallery/image_02.png

Element 2
https://yourname.github.io/vr-gallery/image_03.png

Unused slots can remain empty.

---

## Target Renderer

Assign the mesh renderer that will display the image.

Example:

PicturePlane

---

## Texture Property

Normally:

_MainTex

---

## Caption Text (Optional)

Assign a TextMeshProUGUI component.

The gallery will automatically display captions from the manifest.

---

## Loading Texture (Optional)

Displayed while downloading.

---

## Failed Texture (Optional)

Displayed if a download fails.

---

# Material Setup

The gallery requires a material that accepts textures through:

_MainTex

Recommended shader:

VRGallery/Quest Friendly Unlit MainTex

Alternative:

Unlit/Texture

Avoid particle shaders unless specifically desired.

---

# Previous / Next Buttons

Create two TextMeshPro UI buttons.

Add:

GalleryUIButton

to each button.

Assign:

Gallery = RemoteGalleryManifestSlideshow

Configure:

Previous Button

Is Next Button = False

Next Button

Is Next Button = True

Button OnClick Event:

UdonBehaviour
→ SendCustomEvent(string)
→ PressButton

---

# Random Start Image

Enable:

Random Start Image

The gallery will begin on a random slide each time the world loads.

Disable if you always want to start from slide 0.

---

# Auto Slideshow

Each manifest line controls its own duration.

Example:

0|Welcome|5
1|Artwork|20
2|Photography|10

This displays:

Welcome for 5 seconds
Artwork for 20 seconds
Photography for 10 seconds

---

# Adding New Images

Upload:

image_04.png

Add its URL to Unity:

Element 3

https://yourname.github.io/vr-gallery/image_04.png

Add to manifest:

3|New Image|10

Rebuild and upload world.

After that, captions, order and timing can be changed remotely through the manifest.

---

# Common Problems

## Image Does Not Appear

Check:

* Image URL works in browser
* Renderer assigned
* Correct material assigned
* Correct texture property

Usually:

_MainTex

---

## Gallery Stays On First Image

Check:

* Manifest formatting
* Image indexes are valid
* No missing URLs

Example:

0|Image One|10
1|Image Two|10

Do not reference image indexes that do not exist.

---

## Button Does Nothing

Check:

Button OnClick:

UdonBehaviour
→ SendCustomEvent(string)
→ PressButton

Check:

Gallery reference assigned

Check:

Is Next Button setting

---

## Manifest Not Loading

Open:

https://yourname.github.io/vr-gallery/manifest.txt

in a browser.

You should see plain text.

If you see 404, the file path is incorrect.

---

# Recommended Workflow

1. Upload new image to GitHub
2. Add image URL to Unity
3. Add image to manifest
4. Upload world

After deployment:

* Reorder slides via manifest
* Change captions via manifest
* Change durations via manifest

without touching the world again.

This allows the gallery content to evolve independently from the VRChat world itself.

