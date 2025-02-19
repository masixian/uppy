---
type: docs
order: 2
title: "File Input"
module: "@uppy/file-input"
permalink: docs/file-input/
alias: docs/fileinput/
category: "Sources"
tagline: "even more plain and simple, just a button"
---

`@uppy/file-input` is the most barebones UI for selecting files — it shows a single button that, when clicked, opens up the browser’s file selector.

```js
import FileInput from '@uppy/file-input'

uppy.use(FileInput, {
  // Options
})
```

<a class="TryButton" href="/examples/xhrupload/">Try it live</a>

The `@uppy/xhr-upload` example uses `@uppy/file-input` with the [`pretty`](#pretty-true) option enabled.

## Installation

This plugin is published as the `@uppy/file-input` package.

Install from NPM:

```shell
npm install @uppy/file-input
```

In the [CDN package](/docs/#With-a-script-tag), the plugin class is available on the `Uppy` global object:

```js
const { FileInput } = Uppy
```

## CSS

The `@uppy/file-input` plugin includes some basic styles for use with the [`pretty`](#pretty-true) option, like shown in the [example](/examples/xhrupload). You can also choose not to use it and provide your own styles instead.

```js
import '@uppy/core/dist/style.css'
import '@uppy/file-input/dist/style.css'
```

Import general Core styles from `@uppy/core/dist/style.css` first, then add the File Input styles from `@uppy/file-input/dist/style.css`. A minified version is also available as `style.min.css` at the same path. The way to do import depends on your build system.

## Options

The `@uppy/file-input` plugin has the following configurable options:

```js
uppy.use(FileInput, {
  target: null,
  pretty: true,
  inputName: 'files[]',
  locale: {
  },
})
```

> Note that certain [restrictions set in Uppy’s main options](/docs/uppy#restrictions), namely `maxNumberOfFiles` and `allowedFileTypes`, affect the system file picker dialog. If `maxNumberOfFiles: 1`, users will only be able to select one file, and `allowedFileTypes: ['video/*', '.gif']` means only videos or gifs (files with `.gif` extension) will be selectable.

### `id: 'FileInput'`

A unique identifier for this plugin. It defaults to `'FileInput'`. Use this if you need to add several `FileInput` instances.

### `target: null`

DOM element, CSS selector, or plugin to mount the file input into.

### `pretty: true`

When true, display a styled button (see [example](/examples/xhrupload)) that, when clicked, opens the file selector UI. When false, a plain old browser `<input type="file">` element is shown.

### `inputName: 'files[]'`

The `name` attribute for the `<input type="file">` element.

### `locale: {}`

<!-- eslint-disable no-restricted-globals, no-multiple-empty-lines -->

```js
export default {
  strings: {
    // The same key is used for the same purpose by @uppy/robodog's `form()` API, but our
    // locale pack scripts can't access it in Robodog. If it is updated here, it should
    // also be updated there!
    chooseFiles: 'Choose files',
  },
}

```

## Custom file input

If you don’t like the look/feel of the button rendered by `@uppy/file-input`, feel free to forgo the plugin and use your own custom button on a page, like so:

```html
<input type="file" id="my-file-input">
```

Then add this JS to attach it to Uppy:

```js
const uppy = new Uppy(/* ... */)
const fileInput = document.querySelector('#my-file-input')

fileInput.addEventListener('change', (event) => {
  const files = Array.from(event.target.files)

  files.forEach((file) => {
    try {
      uppy.addFile({
        source: 'file input',
        name: file.name,
        type: file.type,
        data: file,
      })
    } catch (err) {
      if (err.isRestriction) {
        // handle restrictions
        console.log('Restriction error:', err)
      } else {
        // handle other errors
        console.error(err)
      }
    }
  })
})

// it’s probably a good idea to clear the `<input>`
// after the upload or when the file was removed
// (see https://github.com/transloadit/uppy/issues/2640#issuecomment-731034781)
uppy.on('file-removed', () => {
  fileInput.value = null
})

uppy.on('complete', () => {
  fileInput.value = null
})
```
