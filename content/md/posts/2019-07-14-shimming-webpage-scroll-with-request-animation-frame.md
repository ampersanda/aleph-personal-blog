{:title "Shim webpage scroll"
 :layout :post
 :author "Mochamad Lucky Pradana"   
 :tags  ["animation" "javascript" "event-scroll"]
 :toc false}

![Mouse scroll animation](/img/mouse-scroll.gif)

Some sites that I found amazing inside [Awwwards](http://awwwards.com/), are using this method from shimming the scroll of their webpage.

`requestAnimationFrame` is special function for your animation to work, basically you use loop to make changes every few milliseconds. So it’s basic API for use with animation, whether that be DOM-based styling changes, canvas or WebGL. 

The basic HTML structure

```html
<html>
  <!-- ...  -->
  <body>
    <!-- #app will be used for wrapper of your website content -->
    <div id="app">
      <!-- content will be here -->
    </div>
  </body>
  <!-- ...  -->
</html>
```

and the javascript

```js
document.addEventListener('DOMContentLoaded', function() {
    // get the #app
    let wrap = document.getElementById('app');

    // set the styles of #app
    wrap.style.position = 'fixed';
    wrap.style.width = '100%';
    wrap.style.top = '0';
    wrap.style.left = '0';

    // initialize #app position to the window
    // on top of page
    wrap.style.transform = 'translateY(0)'; // you can also use top
});
```
From above code, we make position of `#app` div `fixed` , it's because actually we will simulate the scroll animation using CSS `transform: translateY()` or `top` animation (I use `transform` in this post).

***

## Getting the scroll progress
> Our `#app` has fixed position which means the body doesn’t care about its height anymore, this will deactivate `scroll` event and remove scrollbar from browser.  

So we have to create **an empty div** which has the height of the `#app`.

```js
let fakeDiv = document.createElement('div');
fakeDiv.style.height = wrap.clientHeight + 'px';
document.body.appendChild(fakeDiv);
```

***

## Updating the scroll progress

```js
let update = function () {
  window.requestAnimationFrame(update);

  if (Math.abs(scrollTop - tweened) > 0) {
    // you can change `.072` for the acceleration of scroll
    let top = tweened += .072 * (scrollTop - tweened), // update value of Y translation 
        wt = wrap.style.transform = `translateY(${(top * -1)}px)`;
  }
};

// optional function for adding event
let listen = function (el, on, fn) {
    (el.addEventListener || (on = 'on' + on) && el.attachEvent)(on, fn, false);
};

let scroll = function () {
  scrollTop = Math.max(0, document.documentElement.scrollTop || window.pageYOffset || 0);
};

listen(window, 'scroll', scroll);

// trigger the update function
update();
```
 
>Note: you need to update the height of the **fake scroll div** when resize the page

That's it. [Demo can be accessed here.](https://ampersanda-demo-rafscroll.netlify.com/)

And here's the [complete code](https://github.com/ampersanda/shimming-page-example/blob/master/index.html).

Thank you for taking time to read this article.

Happy coding 😊 
