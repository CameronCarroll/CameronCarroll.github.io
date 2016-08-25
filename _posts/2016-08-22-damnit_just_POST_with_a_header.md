---
title: Damnit just POST with a header
category: programming
layout: back
month_year: Summer 2016
---

All I wanted
--------------
Was to be able to POST from a web form with a custom authorization header. Is that so much to ask? BTW I don't know anything about Javascript, so this will be fun.

<blockquote> No, it's not possible. [To set a custom header on a request in HTML.] - <a href="http://stackoverflow.com/questions/3047711/custom-http-request-headers-in-html">StackOverflow</a></blockquote>

Apparently there was this cool `<meta http-equiv>` tag which was supposed to tell the server to add something to the headers on its end. But the cost of parsing for this tag is too great, and no web server actually supports it.

<blockquote> Servers were supposed to parse HTML, extract meta tag and send it in real HTTP headers. As you can guess, this isn't implemented in any major web server. - <a href="http://stackoverflow.com/questions/5236900/meta-http-equiv-is-it-sent-as-part-of-an-http-header-or-does-the-client-parse">StackOverflow Again</a></blockquote>

So I guess we have to use Javascript after all.

<blockquote> If you want to add a custom header (or set of headers) to an individual request then just add the headers property - <a href="http://stackoverflow.com/questions/7686827/how-can-i-add-a-custom-http-header-to-ajax-request-with-js-or-jquery">StackOverflow again :)</a></blockquote>

```js
// "Just use this" says above StackOverflow post.
// Request with custom header
$.ajax({
  url: 'foo/bar',
  headers: { 'x-my-custom-header': 'some value' }
});
// Okay, great, what do I do with that though?
```

So I guess this was an exercise in reading StackOverflow posts. We hijack the submit method using javascript, and give it this ajax jscript thing which just mirrors a bunch of stuff to build the request.

```html
<form id="message" action="/entry" method="POST">
    ...
</form>

<script type="text/javascript">
    var frm = $('#message');
    frm.submit(function (ev) {
        $.ajax({
            type: frm.attr('method'),
            url: frm.attr('action'),
            data: frm.serialize(),
            headers: { 'Authentication': <%= @hmac_message %>}
            success: function (data) {
                alert('frosty');
            }
        });

        ev.preventDefault();
    });
</script>
```
<a href="http://stackoverflow.com/a/11855073"> (source)

That was an okay start, except I had to wrap it in a ```$(document).ready(function() {});``` in order to get it to do anything. I tried a similar code that chained success as a function call after ajax, but apparently that either has to be a done call (if chaining) or move the success back inside the ajax call. <a href="http://stackoverflow.com/questions/18588861/getting-typeerror-ajax-done-is-not-a-function-ajax-jquery"> (source) </a>

I wasn't 100% sure if you could just drop an erb interpolation into a javascript. I guess there's no reason why not but it turns out: You sure can.

I found <a href="http://stackoverflow.com/questions/10093053/add-header-in-ajax-request-with-jquery">yet another code</a> that suggested doing a beforeSend instead of just a headers: in ajax:

```js
beforeSend: function (request) {
  request.setRequestHeader("AUTHORIZATION", "<%= @auth_string %>");
}
```

Although I just realized that I'm dumb, and I was setting the header as authentication instead of authorization. Also the HTTP_ is implied when setting the header name. Also I'm dumb because my other variable is @auth_string and not @hmac_message. Just not paying attention...

Anyway after some testing, that beforeSend is entirely unnecessary and the original headers: format works just fine. The document ready call is necessary.

Lastly I figured I would note that the ev.preventDefault call is a best practice alternative to return false, for reasons I don't really care to remember.

Oh BTW it's working, and I have my calculated HMAC string on the other side, so thanks for all your help, me.

```js
<script type="text/javascript">
$(document).ready(function(){
  $('#form').submit(function(ev) {
    var valuesToSubmit = $(this).serialize();
    $.ajax({
      type: "POST",
      url: $(this).attr('action'), //sumbits it to the given url of the form
      data: valuesToSubmit,
      headers: { 'AUTHORIZATION': "<%= @auth_string %>"},
      success: function(response) {
         console.log("succeeded!");
      }
    });
    ev.preventDefault(); // prevents normal behaviour
  });
});
</script>
```
