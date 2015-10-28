# Reactive-lite

[![Build Status](https://secure.travis-ci.org/chemzqm/reactive-lite.png)](http://travis-ci.org/chemzqm/reactive-lite)

This component is carefully designed and heavily tested, but bugs always exists, feel free to fire a issue.

TODO: Bindings analysis and reuse

## Features

* Flexible binding fashion, including text-interpolation, format and render for different usage
* Bind attribute (especially `src` `herf`) and event handler easily
* Custom binding API available
* Performance concerned, use `textContent` for format and text interpolation
* Safety concerned, values are escaped by default, unescape variable by using `{!name}`
* ~~Reusable binding for list of reactive works much faster~~
* Easily works with checkbox(es) and select element

## Install

    npm i reactive-lite

## Basic

* **interpolate** `<span>{first} {last}</span>`
* **format** `<div data-format="formatMoney">{money}</div>` with function like:

``` js
function(money) { return '$' + money }
```

No context for format, the result is rendered with `el.textContent`

* **render** `<div data-render="checkActive" >Show on active</div>` with function like

``` js
function(model, el) { el.style.display = model.active ?'block': 'none'}
```

The function is called with `model` `element`, context is the delegate if it's from delegate object

* **attr-interpolation** `<a data-href="http://github.com/{uid}">{name}</a>`
* **event binding** `<button on-click="onBtnClick">click me</button>` function is called with `event`, `model` and `element`
* **checked/selected stat** `<input type="checkbox" name="active" data-checked="active"/>`

**NOTICE**:

* Interpolation and format result is rendered with textContent, just for performance.

## Usage

``` js
var reactive = require('reactive-lite')
var el = document.getElementById('user')
var domify = require('domify')

reactive(el, model)
document.body.appendChild(reactive.el)
```
## API

### Reactive(el, model, [option])

* `el` could be element or html template string
* `model` contains attributes for binding to the element and binding functions, should emit `change [name]` event
* `option` is optional object contains config
* `option.delegate` contains event handler and/or format and render function(s)
* `option.bindings` contains bindings for this reactive-lite only

Binding function are searched on `model` first, if not, search delegate instead, throw error if not found

### .remove()

Unbind all events and remove `reactive.el`

## Advance usage

### Custom binding

* Create `data-visible` binding to dynamically display element by check the `prop` value from model

``` html
<div data-visible="active"></div>
```
``` js
var Reactive = require('reactive-lite')
Reactive.createBinding('data-visible', function(prop) {
  this.bind(prop, function(model, el) {
    el.style.display = model[prop] ? 'block' : 'none'
  })
})
```

* Create `data-sum` binding to dynamically sum the properties

``` html
<div data-sum="x,y,z"></div>
```
``` js
var Reactive = require('reactive-lite')
Reactive.createBinding('data-sum', function(prop) {
  var arr = prop.split(',')
  this.bind(arr, function(model, el) {
    var res = arr.reduce(function(pre, v) {
        return pre + Number(v)
    }, 0)
    el.textContent = res
  })
})
```

## Checkbox and select

## Events

* `change`
* `click`
* `tap`
* `dblclick`
* `mousedown`
* `mousaup`
* `mousemove`
* `mouseenter`
* `mouseleave`
* `scroll`
* `blur`
* `focus`
* `input`
* `submit`
* `keydown`
* `keypress`
* `keyup`

## MIT license
Copyright (c) 2015 chemzqm@gmail.com

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
