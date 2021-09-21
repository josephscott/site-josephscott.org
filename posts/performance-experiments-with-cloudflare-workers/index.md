---
title: Performance Experiments With Cloudflare Workers
description: None
date: 2021-09-21
tags:
  - performance
  - cloudflare
  - webpagetest
  - modheader
  - andy-davies
  - andrew-galloni
layout: layouts/post.njk
---

In the last few years both <a href="https://calendar.perfplanet.com/2019/prototyping-optimizations-with-cloudflare-workers-and-webpagetest/">Andrew Galloni</a> and <a href="https://andydavies.me/blog/2020/09/22/exploring-site-speed-optimisations-with-webpagetest-and-cloudflare-workers/">Andy Davies</a> have written about using <a href="https://workers.cloudflare.com/">Cloudflare Workers</a> to make experimental perfomance changes to production sites, and testing those changes with <a href="https://www.webpagetest.org/">WebPageTest</a>.  It was only recently that I finally tried it out, and it really is as good as they made it sound.

If you want to try it out, go read their articles.  That will get you up to speed on how to get it setup and working.  I'm going to put an extra plug in for using the <a href="https://chrome.google.com/webstore/detail/modheader/idgpnmonknjnojddfkpgkljpfnnfcklj?hl=en">ModHeader Chrome extension</a> for testing this out in the browser, which was suggested by Andy Davies.

Below is my Worker code.  I made a few small changes to those listed by Andrew and Andy - to keep things easy I wanted all the lines of code that needed to be altered for each test at the top of the file:

```js/0-8
const site = 'example.com';

const rewriter = new HTMLRewriter()
	.on( 'title', {
		element: el => {
			el.prepend( 'TEST: ' );
		}
	} );

/* *** Nothing to change below this line *** */

addEventListener( 'fetch', event => {
	event.respondWith( handleRequest( event.request ) );
} );

async function handleRequest( request ) {
	const url = new URL( request.url );

	const host = request.headers.get( 'x-host' );
	if ( !host ) {
		return new Response( 'No x-host header', {
			status: 403
		} );
	}

	url.hostname = host;
	const acceptHeader = request.headers.get( 'accept' );
	const bypass = request.headers.get( 'x-bypass' );

	if ( url.pathname === '/robots.txt' ) {
		return new Response( 'User-agent: *\nDisallow: /' );
	}

	const response = await fetch( url.toString(), request );
	if (
		host === site
		&& ( acceptHeader && acceptHeader.indexOf( 'text/html' ) >= 0 )
		&& ( !bypass || ( bypass && bypass.indexOf( 'true' ) === -1 ) )
	) {
		return rewriter.transform( response );
	}

	return response;
}
```

In less than 50 lines of code you have a reverse proxy, and the <a href="https://developers.cloudflare.com/workers/runtime-apis/html-rewriter">HTMLRewriter</a> class provides a number of features to help you in alterning the HTML of the page.  All of that works with confirming results via <a href="https://www.webpagetest.org/">WebPageTest</a>.

Testing perfomance changes is a requirement.  If you don't show that it actually made things faster, then you don't know if it really did.  Too many times I've seen a change made in hopes of making something faster, but it wasn't until it was tested that it turned out to have no impact.  Or in some cases even make things worse.

All of this together is a powerful set of tools for trying out changes to production web sites without having any access to the servers or source code of the site.
