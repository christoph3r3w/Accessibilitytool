  <script>
    import { onMount } from 'svelte';
    import AccessibilityMenu from '$lib/components/AccessibilityMenu.svelte';
    import { profile, language } from '$lib/stores/accessibility';
    import { page } from '$app/stores';
  
    let spoken = false;
  
    function speak(text, lang) {
      if (typeof window === 'undefined') return;
      const synth = window.speechSynthesis;
      if (synth.speaking) synth.cancel();
  
      const utter = new SpeechSynthesisUtterance(text);
      utter.lang = lang === 'nl' ? 'nl-NL' : 'en-US';
      synth.speak(utter);
    }
  
    // Voorleesfunctie activeren bij blind profiel
    $: if ($profile === 'blind' && !spoken) {
      onMount(() => {
        const content = document.body.innerText;
        speak(content, $language);
        spoken = true;
      });
    }
  
    // Reset bij route wissel of profiel wijziging
    $: if ($page.url && $profile !== 'blind') {
      spoken = false;
    }

    import "../app.css";
    
  </script>
  
  <slot />
    
  
  <h1>Welcome to your library project Test AccessibilityTool</h1>
  <p>Create your package using @sveltejs/package and preview/showcase your work with SvelteKit</p>
  <p>Visit <a href="https://svelte.dev/docs/kit">svelte.dev/docs/kit</a> to read the documentation</p>
  
  <AccessibilityMenu />
  