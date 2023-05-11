<script context="module">
    export function load({ url }) {
      const ref = url.searchParams.get('id')
      return {
        props: {
          ref
        }
      };
    }
  </script>
  <script>
    import { onMount } from "svelte";
    import { browser } from '$app/environment';
    onMount(() =>{
        console.log(getUserIdTosearch())
    }) 
    const getUserIdTosearch = () =>{
      if(browser){
          let id = location.href.split("/")[location.href.split("/").length - 1]
          return id
      }
    }
    export let ref;
    console.log(ref)
    // do stuff
  </script>

  <h1>USER</h1>