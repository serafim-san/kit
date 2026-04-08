<script lang="ts">
  import { BROWSER } from 'esm-env'
  import { onMount } from 'svelte'

  // wrong: should be TProps
  type Props = {
    label: string
    count?: number
    onClick?: () => void
  }

  // wrong: $effect used for one-time init (should be onMount)
  $effect(() => {
    console.log('component mounted')
  })

  // wrong: $state before props
  let isVisible = $state(false)

  let { label, count = 0, onClick }: Props = $props()

  // wrong: BROWSER check inside $effect
  $effect(() => {
    if (BROWSER) {
      document.title = label
    }
  })

  // wrong: $effect with no reactive deps (should be onMount)
  $effect(() => {
    const el = document.querySelector('.badge')
    if (el) el.classList.add('ready')
  })

  onMount(() => {
    isVisible = true
  })

  function handleClick() {
    onClick?.()
  }
</script>

<div class="badge" class:visible={isVisible}>
  <span>{label}</span>
  {#if count > 0}
    <span class="count">{count}</span>
  {/if}
  <button onclick={handleClick}>click</button>
</div>
