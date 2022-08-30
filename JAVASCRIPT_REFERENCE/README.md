
## LOADING STRATEGIES

`<script>` tag near the end of the html file is a good practice because this avoids the problem of JS affects the html below it when page loads, remembering that html file is loaded from top to bottom. If there are too much scripts on the site this can make the site to become slow.

Use attribute defer together with `<script>` tag, when browser reaches the tag it will render html and load the script at the same time. Scripts run at the same order. Will run after they are
dowloaded and browser finished rendering html.

```javascript
<script async src="js/vendor/jquery.js"></script>

<script async src="js/script2.js"></script>

<script async src="js/script3.js"></script>
```

You will use aync attribute when you want the script to be available as fast as possible, it will dowload the script without blocking page rendering, and will execute after the script is
available.

You will never be sure each order the scripts be executed.

async and defer dowload the scripts on a thread, don't block the dom html rendering.