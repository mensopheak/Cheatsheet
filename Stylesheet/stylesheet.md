* https://www.pexels.com/ : free stock photos & videos shared by talented creators



### 1. RESET CSS

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
```

* "*" all elements (reset the default styles of the browser)

* Use ```box-sizing: border-box``` to maintain the sizes of the element (padding changed, but defined sizes not changed)

  



### 2. BUTTON CSS

```css
.btn {
  background-color: #4c6ca0;
  color: #ffffff;
  border: none;
  padding: 10px 20px;
  text-decoration: none;
  font-size: 16px;
  border-radius: 5px;
  cursor: pointer;
}

.btn:hover {
  color: #f4f4f4;
  background-color: #446190;
}
```

* Can use both for <a> or <button>
* Hover could lighter or darker





### 3. SIDE MENU

```css
/* CSS */
.side-menu {
    list-style: none;
    padding: 20px;
    border: 1px solid #ddd;
    border-radius: 10px;
    width: 300px;
}

.side-menu li {
    font-size: 18px;
    line-height: 2.4em;
    border-bottom: 1px dotted #ddd;
}

.side-menu li:last-child {
    border: none;
}

.side-menu li a {
    color: #333;
    text-decoration: none;
}

.side-menu li a:hover {
    color: coral;
}
```

```html
<!-- HTML -->
<ul class="side-menu">
  <li><a href="#">Home</a></li>
  <li><a href="#">About</a></li>
  <li><a href="#">Services</a></li>
  <li><a href="#">Contact</a></li>
</ul>
```



### 4. MEDIA QUERY

* One file

```css
/* Smartphone */
@media (max-width: 500px) {}

/* Tablet */
@media (min-width: 501px) and (max-width: 768px) {}

/* Normal */
@media (min-width: 769px) and (max-width: 1200px) {}

/* Normal */
@media (min-width: 769px) and (max-width: 1200px) {}

/* Widescreen */
@media (min-width: 1201px) {}
```



* Seperate css file by device, example use stylesheet in **tablet.css**, if the screen has min-width of 501px and max-width of 768px

```html
<link rel="stylesheet" href="tablet.css" media="screen and (min-width: 501px) and (max-width: 768px)">
```





### Em and Rem Units

* **em** relative to the font-size of the element (2em means 2 times the size of the current font)(**Not recommeded**)

**Confuse 1**

```css
/* current font-size is 10px */
ul {
  font-size:1.5em;
}
```

```html
<ul> 
	<li>1</li> (font-size: 10 * 1.5 = 15px)
  <li>2</li> (font-size: 10 * 1.5 = 15px)
  <li> (font-size: 10 * 1.5 = 15px)
  	<ul>
      <li>1</li> (font-size: 15 * 1.5 = 22.5px)
      <li>2</li> (font-size: 15 * 1.5 = 22.5px)
        <li> (font-size: 15 * 1.5 = 22.5px)
          <ul>
            <li>1</li> (font-size: 22.5 * 1.5 = 33.75px)
            <li>2</li> (font-size: 22.5 * 1.5 = 33.75px)
          </ul>
        </li>
    </ul>
  </li>
</ul>
```

**Confuse 2**

```css
div{
  font-size: 10px;
}
p{
	font-size: 1.2em; (font-size: 10 * 1.2 = 12px)
  padding: 1.5em; (padding: 12 * 1.5 = 18px)
}
```

```html
<div>
  <p> 
    Lorem ipsum dolor sit amet consectetur adipisicing elit. Sunt numquam rem commodi voluptates ab temporibus fugit quis ea facere nihil!
  </p>
</div>
```



* **rem** relative to font-size of the root element (**recommeded**)

```css
html {
  font-size:10px;
}
h3{
  font-size: 2rem; (font-size: 10 * 2 = 20px)
}
p{
  font-size: 2rem; (font-size: 10 * 2 = 20px)
  line-height: 1.5rem; (font-size: 10 * 1.5 = 15px)
}
```

```html
<div>
  <h3>
    Heading 3
  </h3> 
  <p>
    Lorem ipsum dolor sit amet consectetur adipisicing elit. Sunt numquam rem commodi voluptates ab temporibus fugit quis ea facere nihil!
  </p>
</div>
```

> **Tip:** The em and rem units are practical in creating perfectly scalable layout!



### Vh & Vw Units

* **vw** Relative to 1% of the width of the viewport*
* **vh** Relative to 1% of the height of the viewport*

> Viewport = the browser window size. If the viewport is 50cm wide, 1vw = 0.5cm.





### DISPLAY

> With 'display: inline', the width, height, margin-top, margin-bottom, and float properties have no effect.



### POSITIONS

* By default, position of the element is static

> 1. **static** : not effected by TBLR (top, bottom, left, right) properties/values
> 2. **relative** : TBLR values cause element to be moved from its normal position
> 3. **absolute** : positioned relative to its parent element that is positioned "**relative**"
> 4. **fixed** : positioned relative to the viewport
> 5. **sticky** : positioned based on scroll position

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Position</title>
    <style>
        body {
            height: 200vh;
        }

        .box {
            width: 100px;
            height: 100px;
        }

        .container {
            width: 500px;
            height: 500px;
            background: #333;
            position: relative;
        }

        #box-1 {
            background: red;
            position: relative;
            top: 50px;
            left: 50px;
            z-index: 1;
        }

        #box-2 {
            background: blue;
            top: 100px;
            left: 100px;
            position: absolute;
        }

        #box-3 {
            background: green;
            bottom: 100px;
            right: 100px;
            position: absolute;
        }

        #box-4 {
            background: yellow;
            position: fixed;
        }

        #box-5 {
            background: pink;
            position: sticky;
            top: 0;
        }
    </style>
</head>

<body>
    <div id="box-1" class="box"></div>
    <div class="container">
        <div id="box-2" class="box"></div>
        <div id="box-3" class="box"></div>
    </div>
    <div id="box-4" class="box"></div>
    <div id="box-5" class="box"></div>
</body>

</html>
```



### Visibility, Order & Negative Margin

```css
display: none;
```

> will remove the element from the document

```css
visibility: hidden;
```

> just hide but the element is still remain in the document

```css
color: blue !important;
```

> **!important** flag if we want the style to be overrided (example when we use css framework)

> should avoid negative margin :)