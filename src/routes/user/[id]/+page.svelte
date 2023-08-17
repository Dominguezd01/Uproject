<script context="module">
  import { browser } from "$app/environment"
  if (browser) {
    location.href = "/"
  }
  export function load({ url }) {
    const ref = url.searchParams.get("id")
    return {
      props: {
        ref,
      },
    }
  }
</script>

<script>
  import { onMount } from "svelte"
  import { browser } from "$app/environment"
  const getUserIdTosearch = () => {
    if (browser) {
      let id = location.href.split("/")[location.href.split("/").length - 1]
      return id
    }
  }
  export let ref
</script>

<h1>USER</h1>
