---
title: "🏎️ Speed Up Your Website Performance with WebP"
summary: "Use webp for static resources to reduce load times"
date: 2025-05-04
tags:
  - SoftwareEngineering
---
Here's a simple trick to improve the performance and user experience of your website -
convert the images on your website to `webp` format.

## What is WebP?

WebP is a modern image format developed by Google in 2010.

## Why you should use WebP

- **Smaller files**: WebP supports lossy compression which reduces file sizes by 25-34% and lossless compressions with a reduction of 26% in file sizes.
  This means faster page loading, lesser bandwidth consumption and a better user experience.
- **Excellent image quality**: WebP uses superior compression techniques to reduce file sizes without significant loss of quality.
- **Support for animations and transparency**: WebP isn't just for static images. It is an alternative to GIFs/APNGs (animations) and PNGs (alpha transparency), enabling rich content on your website.
- **Support for modern browsers**: All modern browsers such as Chrome, Firefox, Edge and Safari support WebP. You might also want to consider providing a fallback option using image formats such as `jpg` for older browsers.

<br/>

I've converted my images to WebP for my blog, and here's the results for my blog's landing page:
- Before WebP: 8MB in 220ms
- After WebP: 1.2MB in 180ms

From the above stats, there was a staggering **85% reduction in file sizes** and a **20% reduction of loading time**. 
What a great improvement!
(Results may vary depending on the ratio of text content to images on your website.)

A word of caution:
For vector images (icons, logos, etc.), you should still stick to the `svg` format since that format is already optimized for vector graphics. 
Video formats such as `mp4` or `ogg` might also fare better than `webp` depending on the encoding and compression used.

<br/>

Still hosting `bpm`, `jpeg`, `gif`, `png` on your website?
It's time to make the switch to `webp`!