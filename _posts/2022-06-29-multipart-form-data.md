---
title: "Multipart form data"
date: 2022-06-29T11:34:00+09:00
categories:
  - Memo
tags:
  - HTTP
  - multipart form data
# header:
#   teaser: /assets/images/code.jpg
---
## Multipart form data

The ENCTYPE attribute of `<form>` tag specifies the method of encoding for the form data. It is one of the two ways of encoding the HTML form. It is specifically used when file uploading is required in HTML form. It sends the form data to server in multiple parts because of large size of file.

```html
<form action="login.php" method="post" 
    enctype="multipart/form-data">
</form>
```
