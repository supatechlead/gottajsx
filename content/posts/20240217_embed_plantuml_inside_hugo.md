--- 
draft : false
date : 2024-02-16T14:37:18+01:00
title : "How to Embed Plantuml inside Hugo Article"
tags : ["plantuml","Hugo"]
showTableOfContents : true
type: "post"
---

I often need to embed diagrams into documentation but I dislike having to keep track of separate files for the image. I went looking for a way to embed them as “code” in the document and found GitLab’s ability to embed PlantUML in Markdown content including READMEs, issues, MRs, etc. I want to be able to do the same with Hugo.

I spun up a Hugo Shortcode to add [support](https://gohugo.io/templates/shortcode-templates/) for `{{< plantuml >}} ... {{< /plantuml >}}` in pages where the source for the diagram is in the middle.

Stick this in a Hugo Markdown page…
```plantuml
{{< plantuml id="eg" >}}
Alice -> Bob: test
{{< /plantuml >}}
```
... and it becomes this when rendered:
<center>
{{< plantuml id="eg" >}}
Alice -> Bob: test
{{< /plantuml >}}
<center>

The source for the shortcode is below. I also put a copy of `rawdeflate.js` from [here](https://github.com/johan/js-deflate) in `static/js/rawdeflate.js`. 

```html
l-{{ .Get "id" }}"/>
<script src="/js/rawdeflate.js"></script>
<script>
  // Based on https://plantuml.com/code-javascript-asynchronous

  function encode64(data) {
    r = "";
    for (i=0; i<data.length; i+=3) {
      if (i+2==data.length) {
        r +=append3bytes(data.charCodeAt(i), data.charCodeAt(i+1), 0);
      } else if (i+1==data.length) {
        r += append3bytes(data.charCodeAt(i), 0, 0);
      } else {
        r += append3bytes(data.charCodeAt(i), data.charCodeAt(i+1),
            data.charCodeAt(i+2));
      }
    }
    return r;
  }

  function append3bytes(b1, b2, b3) {
    c1 = b1 >> 2;
    c2 = ((b1 & 0x3) << 4) | (b2 >> 4);
    c3 = ((b2 & 0xF) << 2) | (b3 >> 6);
    c4 = b3 & 0x3F;
    r = "";
    r += encode6bit(c1 & 0x3F);
    r += encode6bit(c2 & 0x3F);
    r += encode6bit(c3 & 0x3F);
    r += encode6bit(c4 & 0x3F);
    return r;
  }

  function encode6bit(b) {
    if (b < 10) {
      return String.fromCharCode(48 + b);
    }
    b -= 10;
    if (b < 26) {
      return String.fromCharCode(65 + b);
    }
    b -= 26;
    if (b < 26) {
      return String.fromCharCode(97 + b);
    }
    b -= 26;
    if (b == 0) {
      return '-';
    }
    if (b == 1) {
      return '_';
    }
    return '?';
  }

  var deflater = window.SharedWorker && new SharedWorker('/js/rawdeflate.js');
  if (deflater) {
    deflater.port.addEventListener('message', done_deflating, false);
    deflater.port.start();
  } else if (window.Worker) {
    deflater = new Worker('/js/rawdeflate.js');
    deflater.onmessage = done_deflating;
  }

  function done_deflating(e) {
    const img = document.getElementById("plantuml-{{ .Get "id" }}");
    img.src = "http://www.plantuml.com/plantuml/img/"+encode64(e.data);
  }

  function compress(s) {
    //UTF8
    s = unescape(encodeURIComponent(s));
  
    if (deflater) {
      if (deflater.port && deflater.port.postMessage) {
        deflater.port.postMessage(s);
      } else {
        deflater.postMessage(s);
      }
    } else {
      setTimeout(function() {
          done_deflating({ data: deflate(s) });
          }, 100);
    }
  }

  compress("{{ .Inner }}")
</script>
```