<script>
   
    import { isLoggedIn } from "../stores/authStore";
  
    import { onMount } from "svelte";
  
    import { getAuth, onAuthStateChanged } from "firebase/auth";
    import { goto } from "$app/navigation";
	import Dashboard from "$lib/components/dashboard.svelte";
  
    onMount(() => {
      const auth = getAuth();
      onAuthStateChanged(auth, (user) => {
        if (user) {
          isLoggedIn.update(() => true);
        } else {
          isLoggedIn.update(() => false);
          goto("/login");
        }
      });
    });
  </script>
 
 <Dashboard />
  <slot></slot>