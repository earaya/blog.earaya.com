---
layout: post
title: "Octopress Review"
date: 2012-07-17 21:55
comments: true
categories: octopress
---

Now that I've been using Octopress for a few weeks, I thought I'd just share some of my initial thoughts on it.

###It Rocks!

1. You can have your blog up and running in under 10 minutes. Furthermore, if you use GitHub pages to host your blog, you get a deployment process out of the box with very little effort.

2. The HTML template that Brandon Mathis has setup is fantastic. My favorite part about the template is how responsive it is, and how well it works in mobile devices. Go ahead, give this site a try on your phone; the template works really well there.

3. The product really makes it a joy for hackers to blog. I've never had so much fun setting up my blog and writing posts.

###Yet

Howerver, there are a few things I wish were a bit different.

1. On version 2.0, it seems like JS is a second class citizen. Scripts are not compiled, minified, and stitched into a single file.

	I know there's an effort to change in this in version 2.1, but I disagree with the direction Brandon has taken. He really should be using [RequireJS](http://requirejs.org).

2. I wish there was tighter integration with S3. There have been several pull requests to add this feature, but they have yet to be merged.

	It'd be nice, for example, to get a deployment process that also uploads gzipped files (sets the headers on S3 for them) and also sets reasonable cache headers for different file types.
	
	Little side note here: [s3cmd](http://s3tools.org/) is awesome. The sync command will compare the files you're trying to upload and only upload changes. Can't recommend it enough.
	
3. A matter of preference, but I wish Mustache or Handlebars was the templating language. Now, to be fair, I think this is a Jekyll dependency, so it's not really an issue with Octopress directly.

###The Veredict

If you love tinkering, Octopress is for you. You'll have an awesome blog quickly and you'll enjoy tweaking it to your heart's content.