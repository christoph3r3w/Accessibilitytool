<script lang="ts">
  import LanguageSelector from './LanguageSelector.svelte';
  import ProfileSelector from './ProfileSelector.svelte';
  import { fade } from 'svelte/transition';
  interface Props {
    children: import('svelte').Snippet<[]>;
  }
  
  let { children }: Props = $props();

  const comment = `it's missing the other a11y components and the styling for the menu and inputs`;
</script>

  <div class="buttonBox">
    <button popovertarget="a11y-menu">♿︎</button>
  </div>

  <div id="a11y-menu" class="menu" popover >
    <h2 class="menu_title">Accessibility Menu</h2>
    <LanguageSelector />
    <ProfileSelector />
    <button popovertarget="a11y-menu" popovertargetaction="hide">x</button>
    {#if children}
      {@render children()}
    {/if}
  </div>

<style>
  :root {
    --btn-top: 4dvh;
    --btn-right: 5dvw;
  }

  .buttonBox {
    display: inline-flex;
    place-content: end;
    width: 100%;
    position: fixed;
    top: var(--btn-top);
    right: var(--btn-right);
    z-index: 1000;
  }

  button {
    anchor-name: --myAnchor;
    z-index: 1000;
    font-size: 1.5rem;
    cursor: pointer;
    aspect-ratio: 1/1;
    width: 3rem;
    border-radius: 50%;
  }

  .menu {
    display: none;
    max-width: min(90dvw,50rem);
    max-height: 50dvh;
    position: fixed;
    top: calc(5dvh + var(--btn-top));
    right: 5dvw;
    left: auto;
    z-index: 10;
    border: 1px solid #ccc;
    border-radius: 0.5rem;
    padding: 1rem;
    margin: 0;
    margin-left: 5dvw;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
    overflow: auto;
    
    transition-behavior: allow-discrete;
    transition: opacity 0.2s ease-out, transform 0.1s ease-in-out,display 0.2s allow-discrete  ;
    outline: olive solid;
  }

  .menu:popover-open {
    display: block;
    opacity: 1;
    transform: translateY(0);

    @starting-style {
      opacity: 0;
      transform: translateY(5px);
    }
  }

  .menu_title {
    font-size: 1.2rem;
  }

  button[popovertargetaction="hide"] {
    display: none;
  }

  /* for the browsers that support anchor() */
  @supports (anchor-name: --myAnchor) {
    [popovertarget] {
      display: inline;
      position: relative;
    }

    .menu[popover] {
      border: 0;
      margin: 0;
      position-anchor: --myAnchor;
      top: calc(anchor(bottom) + 1rem);
      right: anchor(right);
      left: auto;
      outline: orange solid;
    }
  }

  @media (prefers-reduced-motion: reduce) {
    .menu {
      transition: none;
    }
  }

  /* smaller desktop screen */
  @media screen and (width < 600px) {
    .menu {
      position: fixed;
      top: 0;
      right: 0;
      left: 0;
      width: 100%;
      height: fit-content;
      margin: 0;
    }
  }

  /* detection for mobile */
  @media (pointer: coarse) and (hover: none) and (min-resolution: 400dpi),
    screen and (device-width <= 900px) and (width <= 900px) and (orientation: portrait),
    screen and (device-height <= 900px) and (height <= 900px) and (orientation: landscape) {
    .buttonBox {
      position: fixed;
      top: auto;
      bottom: calc(env(safe-area-inset-bottom) + var(--btn-top));
      right: var(--btn-right);
      left: auto;
      width: 100%;
      height: fit-content;
      margin: 0;
    }

    .menu {
      top: auto;
      bottom: 0;
      inset-inline: 4%;
      width: auto;
      margin: 0;
    }

    button[popovertargetaction="hide"] {
      display: inline-flex;
      position: absolute;
      top: var(--btn-top);
      right: 12dvw;
      width: fit-content;
    }

    /* for the browsers that support anchor() */
    @supports (anchor-name: --myAnchor) {
      [popovertarget] {
        display: inline;
        position: relative;
      }

      button[popovertargetaction="hide"] {
        display: none;
      }

      .menu[popover] {
        position-anchor: --myAnchor;
        inset: unset;
        margin-inline: var(--btn-right) max(auto,var(--btn-right));
        width: auto;
        right: anchor(right) !important;
        bottom: calc(anchor(top) + 1rem);
      }
    }
  }
</style>
