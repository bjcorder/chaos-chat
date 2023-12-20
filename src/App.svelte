<script lang="ts">
    import Login from "./lib/Login.svelte";
    import Messages from "./lib/Messages.svelte";
    import { currentUser } from "./lib/pocketbase";
    import Header from "./components/Header.svelte";
    import Footer from "./components/Footer.svelte";

</script>

<Header />




{#if $currentUser}
  <Messages />
  
{/if}

<Login />

<Footer />