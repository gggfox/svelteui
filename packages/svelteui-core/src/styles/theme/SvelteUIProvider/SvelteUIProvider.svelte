<script lang="ts" context="module">
	import { setContext } from 'svelte';
	import { beforeUpdate, get_current_component } from 'svelte/internal';
	import { mergeTheme } from '../';
	import { useSvelteUITheme } from './default-theme';
	import { colorScheme } from './svelteui.stores';
	import { key, useSvelteUIThemeContext } from './svelteui.provider';
	import { createStyles, dark, NormalizeCSS, SvelteUIGlobalCSS } from '../../index';
	import { createEventForwarder, useActions } from '$lib/internal';
	import type { SvelteUITheme } from '../types';
	import type { SvelteUIProviderProps as $$SvelteUIProviderProps } from './svelteui.provider';
	import type { SvelteUIProviderContextType } from './svelteui.provider';
</script>

<script lang="ts">
	export let use: $$SvelteUIProviderProps['use'] = [],
		className: $$SvelteUIProviderProps['className'] = '',
		element: $$SvelteUIProviderProps['element'] = undefined,
		theme: $$SvelteUIProviderProps['theme'] = useSvelteUITheme(),
		styles: $$SvelteUIProviderProps['styles'] = {},
		defaultProps: $$SvelteUIProviderProps['defaultProps'] = {},
		themeObserver: $$SvelteUIProviderProps['themeObserver'] = 'light',
		withNormalizeCSS: $$SvelteUIProviderProps['withNormalizeCSS'] = false,
		withGlobalStyles: $$SvelteUIProviderProps['withGlobalStyles'] = false,
		override: $$SvelteUIProviderProps['override'] = {},
		inherit: $$SvelteUIProviderProps['inherit'] = false;
	export { className as class };

	beforeUpdate(() => {
		const htmlClassList: DOMTokenList = document.documentElement.classList;
		if ($colorScheme === 'dark') htmlClassList.add(dark.className);
		if ($colorScheme === 'light') htmlClassList.remove(dark.className);
	});

	const ctx = useSvelteUIThemeContext();
	const useStyles = createStyles(() => ({ root: {} }));
	const forwardEvents = createEventForwarder(get_current_component());
	const DEFAULT_THEME = useSvelteUITheme();
	const currentTheme = () => {
		if (themeObserver === null) return null;
		return themeObserver === 'light' ? (mergedTheme as unknown as string) : (dark as string);
	};

	$: if (withGlobalStyles) SvelteUIGlobalCSS();
	$: if (withNormalizeCSS) NormalizeCSS();
	$: overrides = {
		themeOverride: inherit ? { ...ctx.theme, ...theme } : theme,
		styles: inherit ? { ...ctx.styles, ...styles } : styles,
		defaultProps: inherit ? { ...ctx.styles, ...defaultProps } : defaultProps
	};
	$: setContext<SvelteUIProviderContextType>(key, {
		theme: overrides.themeOverride as SvelteUITheme,
		styles: {},
		defaultProps: {}
	});
	$: colorScheme.set(themeObserver);
	$: mergedTheme = mergeTheme(DEFAULT_THEME, overrides.themeOverride);
	$: ({ cx, classes } = useStyles(null, { override }));
</script>

<div
	id="SVELTEUI_PROVIDER"
	bind:this={element}
	use:useActions={use}
	use:forwardEvents
	class={cx(className, classes.root, currentTheme())}
	{...$$restProps}
>
	<slot />
</div>
