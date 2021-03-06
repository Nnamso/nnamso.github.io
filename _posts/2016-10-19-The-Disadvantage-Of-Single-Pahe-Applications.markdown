---
layout: post
title:  "The Disadvantages of Single Page Applications"
date:   2016-10-19 9:11:00
categories: tech
description: "The Disadvantages of Single Page Applications"
---

### The disadvantages of Single Page Applications

Single Page Applications (SPAs) have become extremely popular on the web, because they are supposed to provide a more fluid user experience. However, there are many UX and technical problems that arise from architecting websites this way. Before getting to them, let’s first discuss the difference between an SPA and a traditional multi-page website (MPW).

What’s the difference between a Single Page App and an Multi-page Website?
Whilst it’s common to associate MVC, MVVM, XHR, DOM manipulation (and more) with an SPA, this article doesn’t address them because they could also be used in an MPW.

What really defines an SPA is the fact that the routing is handled by the client-side application using Javascript, instead of the server.

This means the application handles the browsing instead of the browser. Attempting to mimic the browser using Javascript is what causes these self-induced issues. Let’s take a look.

1. Navigation and fast back
Browsers store history, meaning pages load quickly when the user presses the back button. SPAs need to recreate this functionality. As Daniel Puplus says in his article:

“Back should be quick; users don’t expect data to have changed much.

“When a user presses the browser’s back button they expect the change to happen quickly and for the page to be in a similar state to how it was last time they saw it.

“In the traditional web model the browser will typically be able use a cached version of the page and linked resources.

“In a naive implementation of a SPA hitting back will do the same thing as clicking a link, resulting in a server request, additional latency, and possibly visual data changes.”

When ‘navigating’, the application will need a method of storing and retrieving ‘pages’ from a cache. Unless of course you want to slow down the loading ‘pages’, which is meant to be a significant benefit of SPAs. Storage options could include memory, local (or session) storage, client-side database and cookies.

Note: The words ‘navigating’ and ‘pages’ are in quotes because SPAs, by definition, don’t have the concept of navigation and pages in the traditional sense. Quotes will be discarded going forward for brevity.

The application will also need to determine when to store and retrieve pages from the cache. Navigation typically utilises pushState or hashchange and the application will need to differentiate between the user changing the URL (via clicking a link or typing a URL in the location bar) or manually hitting back/forward, which is not straightforward.

2. Navigation and remembering scroll history position
Browsers conveniently remember the scroll position of pages you have visited and as Daniel Puplus says in his article:

“Lots of sites get this wrong and it’s really annoying. When the user navigates using the browser’s forward or back button the scroll position should be the same as it was last time they were on the page. This sometimes works correctly on Facebook but sometimes doesn’t. Google+ always seems to lose your scroll position.”

Clicking forward or back should remember the scroll position, but unfortunately, as SPAs rely on faux navigation, this functionality is lost. Upon navigation, the application will need to remember the scroll position so that it can be retrieved later. This is a topic heavily related to “Navigation and fast back” discussed previously.

3. Cancelling navigation
The browser provides a cancel button which when pressed, cancels the loading of the requested page. If a user clicks another link, the browser will cancel the previous request if one is in progress. This is useful for performance and also ensures the user’s internet data allowance isn’t eaten up unnecessarily.

SPA pages are likely to be retrieved via XHR, meaning several requests could be in progress at the same time; the first page request might be loaded last, even though it should have been cancelled out by the second page request. Also, the same link could be clicked twice, meaning the page will be requested (and loaded) twice, which is not efficient and could also cause visual glitches.

The application will need to handle this functionality too. This means exposing a custom cancel button (undesirable), and the duplicate requests need handling as well as cancelling all previous in-progress requests.

4. Navigation and data loss
Browsers normally provide the beforeunload event which allows the application to warn against losing unsaved changes. The application router will need to provide a hook to replicate this functionality i.e. ‘beforeRouting’.

5. Search engine ranking
Whilst some SPAs don’t require SEO, for those that do solutions aren’t straightforward.

6. Loading CSS & Javascript
If an SPA grows to a significant size, loading the entire application on page load may be detrimental to the experience because it’s akin to loading all pages of a website when only the home page was requested. Unfortunately, this leads to attempting to load CSS and JS for certain pages. Script loading is notoriously difficult and contains unreliable hacks which can can be fatal to the reliability of the application.

7. Analytics is harder to implement
Analytics tools will normally track page views and related tools without any extra effort but because an SPA page isn’t really a page, this has to be handled with extra script which is triggered by the application router.

8. Automated functional testing
Whilst you can use Selenium (and other equivalents) to test SPAs, extra effort is required to handle timeouts of XHR calls because there is no signal to Selenium that an XHR call has finished, like there is when a real page finishes loading. This leads to more questions and problems; How long should the timeout be? What happens if it takes longer than normal? The test execution will likely be slower too.

9. Performance problems
Pages are “long lived” increasing the chance of exposing a memory leak due to lack of page reloads. This is known to degrade UX and cause battery drain on mobile devices.

10. Loading indicator issues
When a traditional page is requested, the browser shows a loading indicator custom to that browser. This provides the most accurate indication of when a page will finish loading. With client-side routing, you have to implement your own loading indicator which is detrimental to the User Experience for two reasons:

First, the Javascript implemented loading indicator is inaccurate in terms of progress, because it doesn’t have access to progress information like the browser does and second, it’s disorientating to the user because the individual gets used to the behaviour of their chosen browser and unconsciously and intuitively understands where to look for this information. A Javascript solution is always different.

11. It’s going to fail!
Everyone has Javascript, Right? Wrong. It’s going to fail and because SPAs depend on many different Javascript enhancements, when it does fail it will do so in a fatal way as they tend not to conform to Progressive Enhancement.

Summary
Whilst SPAs are meant to provide a better experience, it’s clear and ironic that they are much harder to design and build with a result that is detrimental to the user.

Javascript is never going to beat the browser at what it does best — browsing. You can still have beautifully rich, enhanced experiences without cramming an entire site into one document.
