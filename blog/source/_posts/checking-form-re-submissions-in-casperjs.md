---
layout: post
title: Checking Form Re-submissions in CasperJS
authorId: simon_timms
date: 2014-04-03
---

ASP.net WebForms has a nasty habit of making developers comfortable with using POST for pretty much everything. When you add a button to the page it is typically managed via a postback. This is okay for the most part but it becomes an issue when using the back button. See HTTP suggests that things which are POSTed are actual data changes where as GETs are side effect free. Most browsers save you from messing up with the back button by simply throwing up an warning[![formresubmission](http://stimms.files.wordpress.com/2014/04/formresubmission.png)](http://stimms.files.wordpress.com/2014/04/formresubmission.png)

This warning is not something we want our users to have to see. Without some understanding of how browsers work it is confusing to understand why users are even seeing this error. On my agenda today is fixing a place where this is occurring.

The first step was to get a test in place which demonstrated the behaviour. Because it is a browser issue I turned to our trusty CasperJS integration tests and wrote a test where I simply navigated to a page and then tried to go back. The test should fail because of the form resubmission.

It didn't.

Turns out that CasperJS(or perhaps PhantomJS on which it is built) is smart enough to simply agree to the form submission. Bummer.

To test this you need to intercept the navigation event and make sure it isn't a form re-submission. This can be done using casper.on

<script src='https://gist.github.com/stimms/9955790.js'></script>

If you add this before the test then navigate around an exception will be thrown any time a form is resubmitted automatically. Once your testing is done you can remove the listener with casper.removeAllListeners.

Now on to actually fixing the codeâ€¦



