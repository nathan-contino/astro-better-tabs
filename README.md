# astro-better-tabs

Zero-JavaScript tabbed content for Astro. Uses the CSS radio button trick to switch panels without any client-side scripting. Labels support inline markdown. Multiple independent tab groups on the same page work without any configuration.

## Installation

```sh
npm install astro-better-tabs
```

## Basic usage

```astro
---
import Tabs from 'astro-better-tabs/Tabs.astro';
import TabItem from 'astro-better-tabs/TabItem.astro';
---

<Tabs>
  <TabItem label="macOS">
    Install with Homebrew: `brew install fusionauth-app`
  </TabItem>
  <TabItem label="Linux">
    Download the `.deb` or `.rpm` package from the releases page.
  </TabItem>
  <TabItem label="Windows">
    Download the `.msi` installer from the releases page.
  </TabItem>
</Tabs>
```

The first tab is selected by default. Clicking a label switches the visible panel. No JavaScript required.

## Props

### `<Tabs>`

No props. Wraps any number of `<TabItem>` children.

### `<TabItem>`

| Prop | Type | Description |
|------|------|-------------|
| `label` | `string` | Tab label text. Supports inline markdown (bold, italic, inline code, links). |

## Markdown labels

Labels are processed as inline markdown, so you can use formatting:

```astro
<Tabs>
  <TabItem label="`curl`">...</TabItem>
  <TabItem label="**Node.js** SDK">...</TabItem>
  <TabItem label="[Python](https://python.org)">...</TabItem>
</Tabs>
```

## Multiple tab groups

Each `<Tabs>` generates its own unique radio group name, so multiple groups on the same page are fully independent:

```astro
<Tabs>
  <TabItem label="Request">...</TabItem>
  <TabItem label="Response">...</TabItem>
</Tabs>

<Tabs>
  <TabItem label="macOS">...</TabItem>
  <TabItem label="Linux">...</TabItem>
</Tabs>
```

## How it works

`TabItem` renders three elements per tab: a hidden `<input type="radio">`, a `<label>`, and a `<div>` panel. All three use CSS `order` to visually sort labels into a row above all panels. The active panel is revealed with `.tab-input:checked + .tab-label + .tab-panel { display: block }`.

`Tabs` renders the slot HTML, then uses a string replacement pass to inject a unique `name` attribute into every radio input and mark the first one `checked`. This happens at build time with no client-side JavaScript.

## Styling

The component uses Tailwind CSS `@apply` directives and expects Tailwind to be configured in the consuming project. Styles are scoped to the component via Astro's `<style>` block.

The active tab label uses `bg-gray-700 text-blue-400` (dark style) and its inner `<span>` gets `text-blue-200`. The inactive labels are `text-gray-500` / `dark:text-gray-400` with hover states. The tab panel background is `bg-white dark:bg-gray-900`.

To customize colors, override these classes in your own stylesheet targeting `.tab-input:checked + .tab-label` and `.tab-input:checked + .tab-label + .tab-panel`.
