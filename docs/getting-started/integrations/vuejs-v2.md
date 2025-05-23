---
menu-title: Vue.js 2.x
meta-title: Vue.js 2.x rich text editor component | CKEditor 5 documentation
category: installation
order: 40
---

{@snippet installation/integrations/framework-integration}

# Vue.js 2.x rich text editor component

<p>
	<a href="https://www.npmjs.com/package/@ckeditor/ckeditor5-vue2" target="_blank" rel="noopener">
		<img src="https://badge.fury.io/js/%40ckeditor%2Fckeditor5-vue2.svg" alt="npm version" loading="lazy">
	</a>
</p>

<info-box warning>
	This guide is about the CKEditor&nbsp;5 integration with Vue.js 2.x. However, Vue 2 has reached EOL and is no longer actively maintained. To learn more about the integration with Vue.js 3+, check out the {@link getting-started/integrations/vuejs-v3 "Rich text editor component for Vue.js 3+"} guide.
</info-box>

Vue.js is a versatile framework for building web user interfaces. CKEditor&nbsp;5 provides the official Vue component you can use in your application.

<info-box>
	The {@link features/watchdog watchdog feature} is available for the {@link getting-started/integrations/react React} and {@link getting-started/integrations/angular Angular} integrations, but is not supported in Vue yet.
</info-box>

## Quick start

### Using CKEditor&nbsp;5 Builder

The easiest way to use CKEditor 5 in your Vue application is by configuring it with [CKEditor&nbsp;5 Builder](https://ckeditor.com/builder?redirect=docs) and integrating it with your application. Builder offers an easy-to-use user interface to help you configure, preview, and download the editor suited to your needs. You can easily select:

* the features you need,
* the preferred framework (React, Angular, Vue or Vanilla JS),
* the preferred distribution method.

You get ready-to-use code tailored to your needs!

### Installing from npm

This guide assumes you already have a Vue project. First, install the CKEditor 5 packages:

* `ckeditor5` – package with open-source plugins and features.
* `ckeditor5-premium-features` – package with premium plugins and features.

```bash
npm install ckeditor5 ckeditor5-premium-features
```

Depending on your configuration and chosen plugins, you may need to install the first or both packages.

Then, install the [CKEditor 5 WYSIWYG editor component for Vue 2](https://www.npmjs.com/package/@ckeditor/ckeditor5-vue2):

```bash
npm install @ckeditor/ckeditor5-vue2
```

To create an editor instance, you must first import the editor and the component modules into the root file of your application (for example, `main.js` when generated by `create-vue`).

```js
import Vue from 'vue';
import CKEditor from '@ckeditor/ckeditor5-vue2';
import App from './App.vue';

Vue.use( CKEditor );

new Vue( { render: ( h ) => h( App ) } ).$mount( '#app' );
```

Use the `<ckeditor>` component inside the template tag. The below example shows how to use the component with open-source and premium plugins.

```html
<template>
	<div id="app">
		<ckeditor :editor="editor" v-model="editorData" :config="editorConfig"></ckeditor>
	</div>
</template>

<script>
import { ClassicEditor, Bold, Essentials, Italic, Mention, Paragraph, Undo } from 'ckeditor5';
import { SlashCommand } from 'ckeditor5-premium-features';

import 'ckeditor5/ckeditor5.css';
import 'ckeditor5-premium-features/ckeditor5-premium-features.css';

export default {
	name: 'app',
	data() {
		return {
			editor: ClassicEditor,
			editorData: '<p>Hello from CKEditor 5 in Vue 2!</p>',
			editorConfig: {
				toolbar: {
                    items: [ 'undo', 'redo', '|', 'bold', 'italic' ],
                },
				plugins: [
                    Bold, Essentials, Italic, Mention, Paragraph, SlashCommand, Undo
                ],
				licenseKey: 'your-license-key',
				mention: { 
					// Mention configuration
				}
			}
		};
	}
}
</script>
```

### Using the component locally

If you do not want the CKEditor component to be enabled globally, you can skip the `Vue.use( CKEditor )` part entirely. Instead, configure it in the `components` property of your view.

```html
<template>
	<div id="app">
		<ckeditor :editor="editor" v-model="editorData" :config="editorConfig"></ckeditor>
	</div>
</template>

<script>
import CKEditor from '@ckeditor/ckeditor5-vue2';
import { Bold, ClassicEditor, Essentials, Italic, Paragraph } from 'ckeditor5';

import 'ckeditor5/ckeditor5.css';

export default {
	name: 'app',
	components: {
		ckeditor: CKEditor.component
	},
	data() {
		return {
			editor: ClassicEditor,
			editorData: '<p>Hello from CKEditor 5 in Vue 2!</p>',
			editorConfig: {
				toolbar: {
                    items: [ 'undo', 'redo', '|', 'bold', 'italic' ],
                },
                plugins: [ Bold, Essentials, Italic, Paragraph ],
			}
		};
	}
}
</script>
```

## Component directives

### `editor`

This directive specifies the editor to be used by the component. It must directly reference the editor constructor to be used in the template.

```html
<template>
		<div id="app">
			<ckeditor :editor="editor" ></ckeditor>
		</div>
</template>

<script>
	import { ClassicEditor } from 'ckeditor5';

	export default {
		name: 'app',
		data() {
			return {
				editor: ClassicEditor,

				// ...
			};
		}
	}
</script>
```

### `tag-name`

By default, the editor component creates a `<div>` container which is used as an element passed to the editor (for example, {@link module:editor-classic/classiceditorui~ClassicEditorUI#element `ClassicEditor#element`}). The element can be configured, so for example to create a `<textarea>`, use the following directive:

```html
<ckeditor :editor="editor" tag-name="textarea"></ckeditor>
```

### `v-model`

A [standard directive](https://vuejs.org/v2/api/#v-model) for form inputs in Vue. Unlike [`value`](#value), it creates a two–way data binding, which:

* Sets the initial editor content.
* Automatically updates the state of the application as the editor content changes (for example, as the user types).
* Can be used to set the editor content when necessary.

```html
<template>
	<div id="app">
		<ckeditor :editor="editor" v-model="editorData"></ckeditor>
		<button v-on:click="emptyEditor()">Empty the editor</button>

		<h2>Editor data</h2>
		<code>{{ editorData }}</code>
	</div>
</template>

<script>
	import { ClassicEditor } from 'ckeditor5';

	export default {
		name: 'app',
		data() {
			return {
				editor: ClassicEditor,
				editorData: '<p>Content of the editor.</p>'
			};
		},
		methods: {
			emptyEditor() {
				this.editorData = '';
			}
		}
	}
</script>
```

In the above example, the `editorData` property will be updated automatically as the user types and changes the content. It can also be used to change (as in `emptyEditor()`) or set the initial content of the editor.

If you only want to execute an action when the editor data changes, use the [`input`](#input) event.

### `value`

Allows a one–way data binding that sets the content of the editor. Unlike [`v-model`](#v-model), the value will not be updated when the content of the editor changes.

```html
<template>
	<div id="app">
		<ckeditor :editor="editor" :value="editorData"></ckeditor>
	</div>
</template>

<script>
	import { ClassicEditor } from 'ckeditor5';

	export default {
		name: 'app',
		data() {
			return {
				editor: ClassicEditor,
				editorData: '<p>Content of the editor.</p>'
			};
		}
	}
</script>
```

To execute an action when the editor data changes, use the [`input`](#input) event.

### `config`

Specifies the {@link module:core/editor/editorconfig~EditorConfig configuration} of the editor.

```html
<template>
	<div id="app">
		<ckeditor :editor="editor" :config="editorConfig"></ckeditor>
	</div>
</template>

<script>
	import { ClassicEditor } from 'ckeditor5';

	export default {
		name: 'app',
		data() {
			return {
				editor: ClassicEditor,
				editorConfig: {
					toolbar: [ 'bold', 'italic', '|', 'link' ]
				}
			};
		}
	}
</script>
```

### `disabled`

This directive controls the {@link module:core/editor/editor~Editor#isReadOnly `isReadOnly`} property of the editor.

It sets the initial read–only state of the editor and changes it during its lifecycle.

```html
<template>
	<div id="app">
		<ckeditor :editor="editor" :disabled="editorDisabled"></ckeditor>
	</div>
</template>

<script>
	import { ClassicEditor } from 'ckeditor5';

	export default {
		name: 'app',
		data() {
			return {
				editor: ClassicEditor,
				// This editor will be read–only when created.
				editorDisabled: true
			};
		}
	}
</script>
```

## Component events

### `ready`

Corresponds to the {@link module:core/editor/editor~Editor#event:ready `ready`} editor event.

```html
<ckeditor :editor="editor" @ready="onEditorReady"></ckeditor>
```

### `focus`

Corresponds to the {@link module:engine/view/document~Document#event:focus `focus`} editor event.

```html
<ckeditor :editor="editor" @focus="onEditorFocus"></ckeditor>
```

### `blur`

Corresponds to the {@link module:engine/view/document~Document#event:blur `blur`} editor event.

```html
<ckeditor :editor="editor" @blur="onEditorBlur"></ckeditor>
```

### `input`

Corresponds to the {@link module:engine/model/document~Document#event:change:data `change:data`} editor event. See the [`v-model` directive](#v-model) to learn more.

```html
<ckeditor :editor="editor" @input="onEditorInput"></ckeditor>
```

### `destroy`

Corresponds to the {@link module:core/editor/editor~Editor#event:destroy `destroy`} editor event.

**Note:** Because the destruction of the editor is promise–driven, this event can be fired before the actual promise resolves.

```html
<ckeditor :editor="editor" @destroy="onEditorDestroy"></ckeditor>
```

## How to?

### Using the Document editor type

If you use the {@link framework/document-editor Document (decoupled) editor} in your application, you need to {@link module:editor-decoupled/decouplededitor~DecoupledEditor.create manually add the editor toolbar to the DOM}.

Since accessing the editor toolbar is not possible until after the editor instance is {@link module:core/editor/editor~Editor#event:ready ready}, put your toolbar insertion code in a method executed upon the [`ready`](#ready) event of the component, like in the following example:

```html
<template>
	<div id="app">
		<ckeditor :editor="editor" @ready="onReady" ></ckeditor>
	</div>
</template>

<script>
	import { DecoupledEditor, Bold, Essentials, Italic, Paragraph, Undo } from 'ckeditor5';
	import 'ckeditor5/ckeditor5.css'

	export default {
		name: 'app',
		data() {
			return {
				editor: DecoupledEditor,
				// ...
			};
		},
		methods: {
			onReady( editor )  {
				// Insert the toolbar before the editable area.
				editor.ui.getEditableElement().parentElement.insertBefore(
					editor.ui.view.toolbar.element,
					editor.ui.getEditableElement()
				);
			}
		}
	}
</script>
```

### Using the editor with collaboration plugins

We provide a **ready-to-use integration** featuring collaborative editing in a Vue application:

* [CKEditor&nbsp;5 with real-time collaboration features](https://github.com/ckeditor/ckeditor5-collaboration-samples/tree/master/real-time-collaboration-for-vue)

It is not mandatory to build applications on top of the above sample, however, it should help you get started.

### Localization

CKEditor&nbsp;5 supports {@link getting-started/setup/ui-language multiple UI languages}, and so does the official Vue 2 component. Follow the instructions below to translate CKEditor&nbsp;5 in your Vue application.

Similarly to CSS style sheets, both packages have separate translations. Import them as shown in the example below. Then, pass them to the `translations` array inside the `editorConfig` prop in the component.

```html
<template>
	<div id="app">
		<ckeditor :editor="editor" v-model="editorData" :config="editorConfig"></ckeditor>
	</div>
</template>

<script>
import { ClassicEditor, Bold, Essentials, Italic, Paragraph } from 'ckeditor5';
// More imports...

import coreTranslations from 'ckeditor5/translations/es.js';
import premiumFeaturesTranslations from 'ckeditor5-premium-features/translations/es.js';

// Style sheets imports...

export default {
	name: 'app',
	data() {
		return {
			editor: ClassicEditor,
			editorData: '<p>Hola desde CKEditor 5 en Vue 2!</p>',
			editorConfig: {
				toolbar: {
                    items: [ 'undo', 'redo', '|', 'bold', 'italic' ],
                },
                plugins: [ Bold, Essentials, Italic, Paragraph ],
				translations: [ coreTranslations, premiumFeaturesTranslations ]
			}
		};
	}
}
</script>
```

For more information, refer to the {@link getting-started/setup/ui-language "Setting UI language"} guide.

## Contributing and reporting issues

The source code of this component is available on GitHub in [https://github.com/ckeditor/ckeditor5-vue2](https://github.com/ckeditor/ckeditor5-vue2).
