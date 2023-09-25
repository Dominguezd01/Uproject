<script context="module">
  import { browser } from "$app/environment"
  export function load({ url }) {
    const ref = url.searchParams.get("id")
    return {
      props: {
        ref,
      },
    }
  }
</script>
