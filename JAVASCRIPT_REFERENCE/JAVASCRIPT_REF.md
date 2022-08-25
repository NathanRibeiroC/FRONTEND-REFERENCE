
## LOADING STRATEGIES

`<script>` tag near the end of the html file is a good practice because this avoids the problem of JS affects the html below it when page loads, remembering that html file is loaded from top to bottom. If there are too much scripts on the site this can make the site to become slow.

Use attribute defer together with `<script>` tag, when browser reaches the tag it will render html and load the script at the same time.

```javascript
<script async src="js/vendor/jquery.js"></script>

<script async src="js/script2.js"></script>

<script async src="js/script3.js"></script>
```