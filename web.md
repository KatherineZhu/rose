# Web

# Glossary

- just-in-time loading VS lazy loading

## Auto download file

```js
      $a = document.createElement('a');
      $a.setAttribute("href", src);   // src is the url of resource
      $a.setAttribute("download", "");

      evObj = document.createEvent('MouseEvents');
      evObj.initMouseEvent( 'click', true, true, window, 0, 0, 0, 0, 0, false, false, true, false, 0, null);
      $a.dispatchEvent(evObj);

```

or open src in a new tab.

## window.IntersectionObservers

https://developers.google.com/web/updates/2016/04/intersectionobserver

- If you need to observe multiple elements, it is both possible and advised to observe multiple elements using the same IntersectionObserver instance by calling `observe()` multiple times.
- `rootBounds` is the result of calling getBoundingClientRect() on the root element, which is the viewport by default. - `boundingClientRect` is the result of getBoundingClientRect() called on the observed element.
- `intersectionRect` is the intersection of these two rectangles and effectively tells you which part of the observed element is visible.
- `intersectionRatio` is closely related, and tells you how much of the element is visible.
- This means that the call to your provided callback is low priority and will be made by the browser during idle time
- If an iframe observes one of its elements, both scrolling the iframe as well as scrolling the window containing the iframe will trigger the callback at the appropriate times.
- The browser support for IntersectionObservers is still fairly slim, so it won’t work everywhere right off the bat just yet.
