[메인으로 이동](../README.md)

# 📒 js - document.getElementById가 리턴하는 것은?

## 📖 소개

- 자바스크립트에서 document.getElementById('id')가 리턴하는 것에 대해 알고 싶어 정리

## 📖 목차

1. 소개
1. 목차
1. 연관 개념 정리
1. document.getElementById('id') 확인
1. Prototype 확인
1. 참고

## 📖 연관 개념 정리

### DOM

- Document Object Model
- 브라우저의 렌더링 엔진은 HTML 문서를 파싱하여 브라우저가 이해할 수 있는 자료구조인 DOM 생성
- HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API
- 프로퍼티와 메서드를 제공하는 트리 자료구조

## 📖 document.getElementById('id') 확인

### 샘플 소스

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id="my-div">
        <h3>hi</h3>
    </div>
    <script>
        let div = document.getElementById('my-div');
    </script>
</body>
</html>
```

### 크롬 콘솔 체크

`document.getElementById('my-div')`

⇒  

```html
<div id="my-div">
    <h3>hi</h3>
</div>
```

`typeof(document.getElementById('my-div'))`

⇒ 'object'

`document.getElementById('my-div').__proto__`

⇒

```html
HTMLDivElement {Symbol(Symbol.toStringTag): 'HTMLDivElement', onmouseenter: undefined, onmouseleave: undefined, constructor: ƒ}
```

![](md-images/2021-11-06-22-55-04.png)

`Object.keys(document.getElementById('my-div'))`

⇒ [ ]

- prototype에게 상속받은 프로퍼티는 표시되지 않는다.

`Object.values(document.getElementById('my-div'))`

⇒ [ ]

## 📖 Prototype 확인

### Prototype

```js
a = document.getElementById('my-div');
b = Object.getPrototypeOf(a); // HTMLDivElement {Symbol(Symbol.toStringTag): 'HTMLDivElement', onmouseenter: undefined, onmouseleave: undefined, constructor: ƒ}
c = Object.getPrototypeOf(b); // HTMLElement {…}
d = Object.getPrototypeOf(c); // Element {…}
e = Object.getPrototypeOf(d); // Node {…}
f = Object.getPrototypeOf(e); // EventTarget {Symbol(Symbol.toStringTag): 'EventTarget', addEventListener: ƒ, dispatchEvent: ƒ, removeEventListener: ƒ, constructor: ƒ}
g = Object.getPrototypeOf(f); // {constructor: ƒ, __defineGetter__: ƒ, __defineSetter__: ƒ, hasOwnProperty: ƒ, __lookupGetter__: ƒ, …}
h = Object.getPrototypeOf(g); // null

```

### Prototype의 key

```js
Object.keys(b)
// ['align']

Object.keys(c)
// (119) ['title', 'lang', 'translate', 'dir', 'hidden', 'accessKey', 'draggable', 'spellcheck', 'autocapitalize', 'contentEditable', 'isContentEditable', 'inputMode', 'offsetParent', 'offsetTop', 'offsetLeft', 'offsetWidth', 'offsetHeight', 'style', 'innerText', 'outerText', 'onbeforexrselect', 'onabort', 'onblur', 'oncancel', 'oncanplay', 'oncanplaythrough', 'onchange', 'onclick', 'onclose', 'oncontextmenu', 'oncuechange', 'ondblclick', 'ondrag', 'ondragend', 'ondragenter', 'ondragleave', 'ondragover', 'ondragstart', 'ondrop', 'ondurationchange', 'onemptied', 'onended', 'onerror', 'onfocus', 'onformdata', 'oninput', 'oninvalid', 'onkeydown', 'onkeypress', 'onkeyup', 'onload', 'onloadeddata', 'onloadedmetadata', 'onloadstart', 'onmousedown', 'onmouseenter', 'onmouseleave', 'onmousemove', 'onmouseout', 'onmouseover', 'onmouseup', 'onmousewheel', 'onpause', 'onplay', 'onplaying', 'onprogress', 'onratechange', 'onreset', 'onresize', 'onscroll', 'onseeked', 'onseeking', 'onselect', 'onstalled', 'onsubmit', 'onsuspend', 'ontimeupdate', 'ontoggle', 'onvolumechange', 'onwaiting', 'onwebkitanimationend', 'onwebkitanimationiteration', 'onwebkitanimationstart', 'onwebkittransitionend', 'onwheel', 'onauxclick', 'ongotpointercapture', 'onlostpointercapture', 'onpointerdown', 'onpointermove', 'onpointerup', 'onpointercancel', 'onpointerover', 'onpointerout', 'onpointerenter', 'onpointerleave', 'onselectstart', 'onselectionchange', 'onanimationend', 'onanimationiteration', …]

Object.keys(d)
// (129) ['namespaceURI', 'prefix', 'localName', 'tagName', 'id', 'className', 'classList', 'slot', 'attributes', 'shadowRoot', 'part', 'assignedSlot', 'innerHTML', 'outerHTML', 'scrollTop', 'scrollLeft', 'scrollWidth', 'scrollHeight', 'clientTop', 'clientLeft', 'clientWidth', 'clientHeight', 'attributeStyleMap', 'onbeforecopy', 'onbeforecut', 'onbeforepaste', 'onsearch', 'elementTiming', 'onfullscreenchange', 'onfullscreenerror', 'onwebkitfullscreenchange', 'onwebkitfullscreenerror', 'children', 'firstElementChild', 'lastElementChild', 'childElementCount', 'previousElementSibling', 'nextElementSibling', 'after', 'animate', 'append', 'attachShadow', 'before', 'closest', 'computedStyleMap', 'getAttribute', 'getAttributeNS', 'getAttributeNames', 'getAttributeNode', 'getAttributeNodeNS', 'getBoundingClientRect', 'getClientRects', 'getElementsByClassName', 'getElementsByTagName', 'getElementsByTagNameNS', 'hasAttribute', 'hasAttributeNS', 'hasAttributes', 'hasPointerCapture', 'insertAdjacentElement', 'insertAdjacentHTML', 'insertAdjacentText', 'matches', 'prepend', 'querySelector', 'querySelectorAll', 'releasePointerCapture', 'remove', 'removeAttribute', 'removeAttributeNS', 'removeAttributeNode', 'replaceChildren', 'replaceWith', 'requestFullscreen', 'requestPointerLock', 'scroll', 'scrollBy', 'scrollIntoView', 'scrollIntoViewIfNeeded', 'scrollTo', 'setAttribute', 'setAttributeNS', 'setAttributeNode', 'setAttributeNodeNS', 'setPointerCapture', 'toggleAttribute', 'webkitMatchesSelector', 'webkitRequestFullScreen', 'webkitRequestFullscreen', 'ariaAtomic', 'ariaAutoComplete', 'ariaBusy', 'ariaChecked', 'ariaColCount', 'ariaColIndex', 'ariaColSpan', 'ariaCurrent', 'ariaDescription', 'ariaDisabled', 'ariaExpanded', …]

Object.keys(e)
// (47) ['nodeType', 'nodeName', 'baseURI', 'isConnected', 'ownerDocument', 'parentNode', 'parentElement', 'childNodes', 'firstChild', 'lastChild', 'previousSibling', 'nextSibling', 'nodeValue', 'textContent', 'ELEMENT_NODE', 'ATTRIBUTE_NODE', 'TEXT_NODE', 'CDATA_SECTION_NODE', 'ENTITY_REFERENCE_NODE', 'ENTITY_NODE', 'PROCESSING_INSTRUCTION_NODE', 'COMMENT_NODE', 'DOCUMENT_NODE', 'DOCUMENT_TYPE_NODE', 'DOCUMENT_FRAGMENT_NODE', 'NOTATION_NODE', 'DOCUMENT_POSITION_DISCONNECTED', 'DOCUMENT_POSITION_PRECEDING', 'DOCUMENT_POSITION_FOLLOWING', 'DOCUMENT_POSITION_CONTAINS', 'DOCUMENT_POSITION_CONTAINED_BY', 'DOCUMENT_POSITION_IMPLEMENTATION_SPECIFIC', 'appendChild', 'cloneNode', 'compareDocumentPosition', 'contains', 'getRootNode', 'hasChildNodes', 'insertBefore', 'isDefaultNamespace', 'isEqualNode', 'isSameNode', 'lookupNamespaceURI', 'lookupPrefix', 'normalize', 'removeChild', 'replaceChild']

Object.keys(f)
// (3) ['addEventListener', 'dispatchEvent', 'removeEventListener']
```

## 📖 참고

### 책

- 모던 자바스크립트 Deep Dive : 자바스크립트의 기본 개념과 동작 원리

### 사이트

[Why doesn't Object.keys work with DOM nodes?](https://stackoverflow.com/questions/56156183/why-doesnt-object-keys-work-with-dom-nodes)

- Dom nodes에서 Object.keys했을 때 나오지 않는 이유



<br><br>

[메인으로 이동](../README.md)