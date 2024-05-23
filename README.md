# EmojiPicker-Vue3


This is a Vue.js component that provides a text input with an integrated emoji picker. Users can select emojis from different categories and insert them into the text input field. The component is designed to be easily integrated into any form.

## Features

- Emoji picker with categorized emojis
- Toggle functionality for the emoji picker
- Click-to-insert functionality for emojis

## 📦 Installation

To install demo page of the Emoji Picker component, use npm:

```bash
npm install
```

## Usage

### Import the Component

In your parent component, import the `EmojiPicker` component:

```vue
import EmojiPicker from './components/EmojiPicker/EmojiPicker.vue'

<template>
  <div>
    <EmojiPicker :enableSearch="false" :enableFrequentlyUsed="true" />
  </div>
</template>
```

### Props

Prop Name | Type | Default | Description
--- | --- | --- | --- |
enableSearch |	Boolean | true | Enables the emoji search input when set to true.
enableFrequentlyUsed | Boolean | true | Tracks and displays frequently used emojis when set to true.

