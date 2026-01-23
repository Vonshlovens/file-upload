# File Upload Tool - Agent Guidelines

This file provides essential guidance to AI coding assistants when working with the CWL API codebase.

## Specifications

**IMPORTANT:** Before implementing any feature, consult the specifications in `specs/README.md`.

- **Assume NOT implemented.** Many specs describe planned features that may not yet exist in the codebase.
- **Check the codebase first.** Before concluding something is or isn't implemented, search the actual code. Specs describe intent; code describes reality.
- **Use specs as guidance.** When implementing a feature, follow the design patterns, types, and architecture defined in the relevant spec.
- **Spec index:** `specs/README.md` lists all specifications organized by category (core, LLM, security, etc.).

## Svelte5 Tips

| Category | ✅ Do | ❌ Don't |
|----------|------|---------|
| **Reactive State** | Use `$state()` for reactive variables: `let count = $state(0);` | Use plain `let` expecting implicit reactivity: `let count = 0;` |
| **Derived Values** | Use `$derived()` for computed values: `let doubled = $derived(count * 2);` | Use `$:` for derivations: `$: doubled = count * 2;` |
| **Side Effects** | Use `$effect()` for side effects: `$effect(() => { console.log(count); });` | Use `$:` for side effects: `$: console.log(count);` |
| **Component Props** | Use `$props()` with destructuring: `let { name, age } = $props();` | Use `export let` for props: `export let name;` |
| **Prop Renaming** | Use JS destructuring: `let { class: klass } = $props();` | Use Svelte-specific syntax: `export { klass as class };` |
| **Rest Props** | Use spread: `let { foo, ...rest } = $props();` | Use `$$restProps` or `$$props` |
| **Event Handlers** | Use `onclick` as a prop: `<button onclick={handleClick}>` | Use `on:` directive: `<button on:click={handleClick}>` |
| **Component Events** | Pass callback props: `<Child onchange={handleChange} />` | Use `createEventDispatcher()` |
| **Forwarding Events** | Spread handler props: `<button {...rest}>` | Use empty directive: `<button on:click>` |
| **Slotted Content** | Use `{#snippet}` and `{@render}` | Use `<slot>` and `<svelte:fragment>` |
| **Bindable Props** | Use `$bindable()`: `let { value = $bindable() } = $props();` | Expect two-way binding to work automatically on props |
| **SSR with Runes** | Keep `$state()` and `$effect()` in client-only components or use `$effect.client()` | Use `$state()` directly in SSR-rendered component logic |
| **Tailwind Classes** | Use full static class names: `class:bg-blue-500={active}` | Use dynamic string interpolation: `class="bg-{color}-500"` |
| **Vite Plugin Order** | Place `tailwindcss()` **before** `sveltekit()` in `vite.config.js` | Place `sveltekit()` first |
| **CSS Import** | Import `app.css` in `+layout.svelte` | Import CSS in `app.html` |
| **TypeScript** | Use `<script lang="ts">` directly (no preprocessor needed in Svelte 5) | Rely on `svelte-preprocess` for TypeScript |
| **Reusable Logic** | Create `.svelte.js` or `.svelte.ts` modules with runes | Create Svelte stores for shared reactive logic |
| **Migration** | Run `npx sv migrate svelte-5` to auto-migrate components | Manually rewrite everything from scratch |
| **Lifecycle Hooks** | Prefer `$effect()` for mount/update logic | Use `beforeUpdate`/`afterUpdate` (deprecated behavior changed) |
| **State in Derived/Effects** | State created inside `$derived`/`$effect` can be read/written locally | Worry about "unsafe read" warnings in most cases |

## Tailwind 4 Guidelines
| Category | ✅ Do | ❌ Don't |
|----------|------|---------|
| **Installation** | Use `@tailwindcss/vite`, `@tailwindcss/postcss`, or `@tailwindcss/cli` as separate packages | Use `tailwindcss` directly as a PostCSS plugin |
| **CSS Import** | Use `@import "tailwindcss";` | Use `@tailwind base;`, `@tailwind components;`, `@tailwind utilities;` |
| **Configuration** | Configure in CSS with `@theme { }` | Use `tailwind.config.js` (still supported but CSS-first is preferred) |
| **Theme Variables** | Define tokens as CSS variables: `--color-brand: #3b82f6;` | Use JS config `theme.extend.colors` (unless migrating) |
| **Custom Colors** | Use `--color-*` namespace: `--color-primary-500: oklch(0.7 0.15 250);` | Use arbitrary values everywhere without defining theme tokens |
| **Opacity Modifiers** | Use slash syntax: `bg-black/50`, `text-white/75` | Use deprecated `bg-opacity-*`, `text-opacity-*` utilities |
| **Shadows** | Use `shadow-xs`, `shadow-sm` (renamed scale) | Use `shadow-sm` expecting the old v3 size (now `shadow-xs`) |
| **Border Radius** | Use `rounded-xs`, `rounded-sm` (renamed scale) | Use `rounded-sm` expecting the old v3 size (now `rounded-xs`) |
| **Blur** | Use `blur-xs`, `blur-sm` (renamed scale) | Use `blur-sm` expecting the old v3 size (now `blur-xs`) |
| **Ring Width** | Use explicit `ring-3` for 3px ring | Use bare `ring` expecting 3px (now 1px by default) |
| **Outline** | Use `outline-hidden` for accessible hidden outline | Use `outline-none` expecting the old accessible hidden behavior |
| **Border Color** | Explicitly specify: `border border-gray-200` | Rely on implicit `gray-200` default (now `currentColor`) |
| **Gradients** | Use `bg-linear-to-r` for linear gradients | Use `bg-gradient-to-r` (still works but `bg-linear-*` is preferred) |
| **Gradient Resets** | Use `via-none` to unset mid-color in variants | Expect gradients to reset automatically in variants |
| **Content Detection** | Let Tailwind auto-detect from `.gitignore` | Manually configure `content` array (usually unnecessary now) |
| **Source Paths** | Use `@source "../path"` to add extra paths in CSS | Rely solely on JS config for content paths |
| **Ignoring Paths** | Use `@source not "path"` to exclude directories | Manually filter in JS config |
| **Safelisting** | Use `@source inline("class1 class2")` | Use `safelist` in JS config |
| **Custom Utilities** | Use `@utility name { }` in CSS | Use `addUtilities()` in JS plugins (still works) |
| **Custom Variants** | Use `@variant name { }` in CSS | Use `addVariant()` in JS plugins (still works) |
| **Container** | Customize with `@utility container { margin-inline: auto; }` | Use `container.center` or `container.padding` config (removed) |
| **Imports** | Let Tailwind handle `@import` natively | Use `postcss-import` (no longer needed) |
| **Vendor Prefixes** | Let Tailwind handle prefixing automatically | Use `autoprefixer` (no longer needed) |
| **Dark Mode (manual)** | Use `@variant dark (&:where(.dark, .dark *))` in CSS | Use `darkMode: 'class'` in JS config |
| **Multiple CSS Files** | Import Tailwind only once in your main CSS file | Import `@import "tailwindcss"` in multiple files (causes issues) |
| **@apply in Components** | Import your component CSS after Tailwind, or use `@reference` | Expect `@apply` to work in isolated CSS files without setup |
| **Migration** | Run `npx @tailwindcss/upgrade` to auto-migrate | Manually rewrite everything |
| **Browser Support** | Target Safari 16.4+, Chrome 111+, Firefox 128+ | Expect support for older browsers (use v3.4 instead) |