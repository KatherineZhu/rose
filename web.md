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

## webpack VS grunt

- webpack 静态模块打包工具
    - a.js 依赖 b.js, b.js 编译后变为 b'.js, 那么 a'.js 中引用的也要同步更改为 b'.js
    - 基于依赖关系打包

- grunt 自动化工具
    - 可以做打包，但不限于
    - task 都是基于文件的

## 进阶问题

1. js 的 import 和 java 的 import 区别？
2. grunt run task ['a', 'b', 'c'] 的时候，是同步 or 异步？
3. js 加 hash 后，webpack 如何更新对应的 html 文件？
4. webpack 按需加载的实现，好像跟 chunk 有关
5. webpack 是基于依赖关系打包，对于 es6 有 import 和 export，除此之外，还有什么语法能体现依赖关系？
