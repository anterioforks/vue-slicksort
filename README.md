# Vue Slicksort
> A set of component mixins to turn any list into an animated, touch-friendly, sortable list.
> Based on [react-sortable-hoc](https://github.com/clauderic/react-sortable-hoc) by [@clauderic]

[![npm version](https://img.shields.io/npm/v/vue-slicksort.svg)](https://www.npmjs.com/package/vue-slicksort)
[![npm downloads](https://img.shields.io/npm/dm/vue-slicksort.svg)](https://www.npmjs.com/package/vue-slicksort)
[![license](https://img.shields.io/github/license/mashape/apistatus.svg?maxAge=2592000)](https://github.com/Jexordan/vue-slicksort/blob/master/LICENSE)
[![Gitter](https://badges.gitter.im/vue-slicksort/Lobby.svg)](https://gitter.im/vue-slicksort/Lobby?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
![gzip size](http://img.badgesize.io/https://npmcdn.com/vue-slicksort?compression=gzip)

### Examples available here: <a href="#">https://jexordexan.github.io/vue-slicksort/</a>

Features
---------------
* **`v-model` Compatible** – Make any array editable with the `v-model` standard
* **Mixin Components** – Integrates with your existing components
* **Drag handle, auto-scrolling, locked axis, events, and more!**
* **Suuuper smooth animations** – Chasing the 60FPS dream 🌈
* **Horizontal lists, vertical lists, or a grid** ↔ ↕ ⤡
* **Touch support** 👌

Installation
------------

Using [npm](https://www.npmjs.com/package/vue-slicksort):
```
  $ npm install vue-slicksort --save
```

Using yarn:
```
  $ yarn add vue-slicksort
```

Then, using a module bundler that supports either CommonJS or ES2015 modules, such as [webpack](https://github.com/webpack/webpack):

```js
// Using an ES6 transpiler like Babel
import {ContainerMixin, ElementMixin} from 'vue-slicksort';

// Not using an ES6 transpiler
var slicksort = require('vue-slicksort');
var ContainerMixin = slicksort.ContainerMixin;
var ElementMixin = slicksort.ElementMixin;
```

Alternatively, an UMD build is also available:
```html
<script src="vue-slicksort/dist/umd/vue-slicksort.js"></script>
```

Usage
------------
### Basic Example

```js
import Vue from 'vue';
import { ContainerMixin, ElementMixin } from 'vue-slicksort';

const SortableList = {
  mixins: [ContainerMixin],
  template: `
    <ul class="list">
      <slot />
    </ul>
  `,
};

const SortableItem = {
  mixins: [ElementMixin],
  props: ['item'],
  template: `
    <li class="list-item">{{item}}</li>
  `,
};

const ExampleVue = {
  name: 'Example',
  template: `
    <div class="root">
      <SortableList lockAxis="y" v-model="items">
        <SortableItem v-for="(item, index) in items" :index="index" :key="index" :item="item"/>
      </SortableList>
    </div>
  `,
  components: {
    SortableItem,
    SortableList,
  },
  data() {
    return {
      items: ['Item 1', 'Item 2', 'Item 3', 'Item 4', 'Item 5', 'Item 6', 'Item 7', 'Item 8'],
    };
  },
};

const app = new Vue({
  el: '#root',
  render: (h) => h(ExampleVue),
});
```
That's it! Vue Slicksort does not come with any styles by default, since it's meant to enhance your existing components.

<!-- More code examples are available [here](https://github.com/Jexordan/vue-slicksort/blob/master/examples/). -->

Why should I use this?
--------------------
There are already a number of great Drag & Drop libraries out there (for instance, [vuedraggable](https://github.com/SortableJS/Vue.Draggable) is fantastic). If those libraries fit your needs, you should definitely give them a try first. However, most of those libraries rely on the HTML5 Drag & Drop API, which has some severe limitations. For instance, things rapidly become tricky if you need to support touch devices, if you need to lock dragging to an axis, or want to animate the nodes as they're being sorted. Vue Slicksort aims to provide a simple set of component mixins to fill those gaps. If you're looking for a dead-simple, mobile-friendly way to add sortable functionality to your lists, then you're in the right place.

## ContainerMixin

### Props
| Property                     | Type              | Default                                                                                                    | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|:-----------------------------|:------------------|:-----------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `value` *(required)*         | Array             |                                                                                                            | The `value` can be inherited from `v-model` but has to be set to the same list that is rendered with `v-for` inside the `Container`                                                                                                                                                                                                                                                                                                                                    |
| `axis`                       | String            | `y`                                                                                                        | Items can be sorted horizontally, vertically or in a grid. Possible values: `x`, `y` or `xy`                                                                                                                                                                                                                                                                                                                                                                           |
| `lockAxis`                   | String            |                                                                                                            | If you'd like, you can lock movement to an axis while sorting. This is not something that is possible with HTML5 Drag & Drop                                                                                                                                                                                                                                                                                                                                           |
| `helperClass`                | String            |                                                                                                            | You can provide a class you'd like to add to the sortable helper to add some styles to it                                                                                                                                                                                                                                                                                                                                                                              |
| `transitionDuration`         | Number            | `300`                                                                                                      | The duration of the transition when elements shift positions. Set this to `0` if you'd like to disable transitions                                                                                                                                                                                                                                                                                                                                                     |
| `pressDelay`                 | Number            | `0`                                                                                                        | If you'd like elements to only become sortable after being pressed for a certain time, change this property. A good sensible default value for mobile is `200`. Cannot be used in conjunction with the `distance` prop.                                                                                                                                                                                                                                                |
| `pressThreshold`             | Number            | `5`                                                                                                        | Number of pixels of movement to tolerate before ignoring a press event.                                                                                                                                                                                                                                                                                                                                                                                                |
| `distance`                   | Number            | `0`                                                                                                        | If you'd like elements to only become sortable after being dragged a certain number of pixels. Cannot be used in conjunction with the `pressDelay` prop.                                                                                                                                                                                                                                                                                                               |
| `useDragHandle`              | Boolean           | `false`                                                                                                    | If you're using the `HandleDirective`, set this to `true`                                                                                                                                                                                                                                                                                                                                                                                                           |
| `useWindowAsScrollContainer` | Boolean           | `false`                                                                                                    | If you want, you can set the `window` as the scrolling container                                                                                                                                                                                                                                                                                                                                                                                                       |
| `hideSortableGhost`          | Boolean           | `true`                                                                                                     | Whether to auto-hide the ghost element. By default, as a convenience, Vue Slicksort List will automatically hide the element that is currently being sorted. Set this to false if you would like to apply your own styling.                                                                                                                                                                                                                                           |
| `lockToContainerEdges`       | Boolean           | `false`                                                                                                    | You can lock movement of the sortable element to it's parent `Container`                                                                                                                                                                                                                                                                                                                                                                                       |
| `lockOffset`                 | `OffsetValue`\* -or- [`OffsetValue`\*, `OffsetValue`\*]                                                              | `"50%"` | When `lockToContainerEdges` is set to `true`, this controls the offset distance between the sortable helper and the top/bottom edges of it's parent `Container`. Percentage values are relative to the height of the item currently being sorted. If you wish to specify different behaviours for locking to the *top* of the container vs the *bottom*, you may also pass in an `array` (For example: `["0%", "100%"]`).                            |
| `shouldCancelStart`          | Function          | [Function](https://github.com/Jexordan/vue-slicksort/blob/master/src/ContainerMixin.js#L41) | This function is invoked before sorting begins, and can be used to programatically cancel sorting before it begins. By default, it will cancel sorting if the event target is either an `input`, `textarea`, `select` or `option`.                                                                                                                                                                                                                                     |
| `getHelperDimensions`        | Function          | [Function](https://github.com/Jexordan/vue-slicksort/blob/master/src/ContainerMixin.js#L49) | Optional `function({node, index, collection})` that should return the computed dimensions of the SortableHelper. See [default implementation](https://github.com/Jexordan/vue-slicksort/blob/master/src/ContainerMixin.js#L49) for more details                                                                                                                                                                                                         |

\* `OffsetValue` can either be a finite `Number` or a `String` made up of a number and a unit (`px` or `%`).
Examples: `10` (which is the same as `"10px"`), `"50%"`

### Events

| Event        | Arguments                                   | Description                                               |
|:-------------|:--------------------------------------------|:----------------------------------------------------------|
| `@sortStart` | `{ event, node, index, collection }`        | Fired when sorting begins.                                |
| `@sortMove`  | `{ event }`                                 | Fired when the mouse is moved during sorting.             |
| `@sortEnd`   | `{ event, newIndex, oldIndex, collection }` | Fired when sorting has ended.                             |
| `@input`     | `newList`                                   | Fired after sorting has ended with the newly sorted list. |

## ElementMixin

### Props
| Property           | Type             | Default | Description                                                                                                                                                                                                                               |
|:-------------------|:-----------------|:--------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| index *(required)* | Number           |         | This is the element's sortableIndex within it's collection. This prop is required.                                                                                                                                                        |
| collection         | Number or String | `0`     | The collection the element is part of. This is useful if you have multiple groups of sortable elements within the same `Container`. [Example](http://Jexordan.github.io/vue-slicksort/#/basic-configuration/multiple-lists) |
| disabled           | Boolean          | `false` | Whether the element should be sortable or not                                                                                                                                                                                             |

FAQ
---------------
<!-- ### Running Examples

In root folder:

```
	$ npm install
	$ npm run storybook
``` -->

### Grid support
Need to sort items in a grid? We've got you covered! Just set the `axis` prop to `xy`. Grid support is currently limited to a setup where all the cells in the grid have the same width and height, though we're working hard to get variable width support in the near future.

### Item disappearing when sorting / CSS issues
Upon sorting, `vue-slicksort` creates a clone of the element you are sorting (the _sortable-helper_) and appends it to the end of the `<body>` tag. The original element will still be in-place to preserve its position in the DOM until the end of the drag (with inline-styling to make it invisible). If the _sortable-helper_ gets messed up from a CSS standpoint, consider that maybe your selectors to the draggable item are dependent on a parent element which isn't present anymore (again, since the _sortable-helper_ is at the end of the `<body>`). This can also be a `z-index` issue, for example, when using `vue-slicksort` within a Bootstrap modal, you'll need to increase the `z-index` of the SortableHelper so it is displayed on top of the modal.

### Click events being swallowed
By default, `vue-slicksort` is triggered immediately on `mousedown`. If you'd like to prevent this behaviour, there are a number of strategies readily available. You can use the `distance` prop to set a minimum distance (in pixels) to be dragged before sorting is enabled. You can also use the `pressDelay` prop to add a delay before sorting is enabled. Alternatively, you can also use the [HandleDirective](https://github.com/Jexordan/vue-slicksort/blob/master/src/HandleDirective.js).


Dependencies
------------
Slicksort has no dependencies. 
`vue` is the only peerDependency

Reporting Issues
----------------
If believe you've found an issue, please [report it](https://github.com/Jexordan/vue-slicksort/issues) along with any relevant details to reproduce it. The easiest way to do so is to fork this [jsfiddle](https://jsfiddle.net/Jexordan/6r7r2cva/).

Asking for help
----------------
Please do not use the issue tracker for personal support requests. Instead, use [Gitter](https://gitter.im/Jexordan/vue-slicksort) or StackOverflow.

Contributions
------------
Yes please! Feature requests / pull requests are welcome.

Thanks
------------
This library is heavily based on [react-sortable-hoc](https://github.com/clauderic/react-sortable-hoc) by Claudéric Demers (@clauderic). A very simple and low overhead implementation of drag and drop that looks and performs great!
