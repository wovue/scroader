# Scroader

The `wovue-scroader` module contains the scroader component.

Install using npm:

```sh
$ npm install --save wovue-scroader
```

**Note** this will only works with `webpack` build system.

## Usage

```js
import { WvScroader } from 'wovue-scroader'

new Vue({
  components: {
    WvScroader
  }
})
```

```html
<wv-scroader>
  <!-- here your header content -->
</wv-scroader>
```

### Props

### Scroader

| Name | Type | Default | Description |
|------|:----:|:--------|:------------|
| headerOffset | Number | `0` | Space between your header and top document. |
| headerTransition | String | `'scroader'` | Transition effect when header is inserted into or removed from the DOM. |
