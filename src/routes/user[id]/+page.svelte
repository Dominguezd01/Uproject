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
    export let ref;
  
    // do stuff
  </script>