---
title: Switch
group: 'svelteuidev-core'
packageGroup: '@svelteuidev/core'
slug: /core/switch/
category: 'inputs'
description: 'Capture boolean input from user'
import: "import { Switch } from '@svelteuidev/core';"
source: 'svelteui-core/src/components/Switch/Switch.svelte'
docs: 'core/switch.md'
---

<script lang="ts">
    import { Demo, SwitchDemos } from '@svelteuidev/demos';
    import { Heading } from 'components';

    const switchCode = `
    <script>
        import { Switch } from '@svelteuidev/core';
    <\/script>

    <Switch label="I agree to sell my privacy" size="md" color="teal"\/>
    <Switch onLabel="ON" offLabel="OFF" label="Setting 1" size="xl" color="pink"\/>
    <Switch checked size="xs"\/>
    `;
    const labelSwitch = `
    <script>
        import { Switch } from '@svelteuidev/core';
    <\/script>

    <Switch size='sm' onLabel="ON" offLabel="OFF" \/>
    <Switch size='md' onLabel="ON" offLabel="OFF" \/>
    <Switch size='lg' onLabel="ON" offLabel="OFF" \/>
    <Switch size='xl' onLabel="ON" offLabel="OFF" \/>
    `;
    const accessibilitySwitch = `
    <script>
        import { Switch } from '@svelteuidev/core';
    <\/script>

    <Switch \/> // -> not ok, input is not labeled
    <Switch label="I agree to everything" \/> // -> ok, input and label is connected
    <Switch aria-label="I agree to everything" \/> // -> ok, label is not visible but will be announced by screen reader
    `;
</script>

<Heading />

## Usage

Switch component is used to enable/disable something, normally used for boolean values or for binary actions. The component dispatches an `on:change` event, and supports a `checked` prop for programmatically setting the checked state.

<Demo demo={SwitchDemos.configurator} />

## Inner labels and Size

There is support for inserting text inside the switch for when it is enabled and/or disabled. It is also possible to control the size of the switch with predefined values: `xs`, `sm`, `md`, `lg`, `xl`.

<Demo demo={SwitchDemos.inner} />

## Accessibility

Switch is a regular input with checkbox type. Provide `aria-label` if Switch is used without `label`:

```svelte
<Switch /> // -> not ok, input is not labeled
<Switch label="I agree to everything" /> // -> ok, input and label is connected
<Switch aria-label="I agree to everything" /> // -> ok, label is not visible but will be announced by
screen reader
```
