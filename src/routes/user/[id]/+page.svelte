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
    console.log(ref)
    // do stuff
  </script>

  <h1>USER</h1>