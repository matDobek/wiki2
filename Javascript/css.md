### What are `@` rules?
Instructions for `css` processor to apply special rules, instead of just applying styles to elements.

```css

/* responsive design */
@media (max-width: 600px) { ... } 

/* allows us to import another style */
@import url("foo.css"); 

/* define custom fonts */
@font-face {  
	font-family: "MyFonterino";
	src: url("helvetica.woff2") format("woff2");
}
body {
  font-family: 'MyFonterino', sans-serif;
}

```

### What is the difference between `"` and `'`?
None
`"` may be more consistent with `html`