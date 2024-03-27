<script>
   import App from './firebase';
    import { isLoggedIn } from "../stores/authStore";
  
    import { onMount } from "svelte";
  
    import { getAuth, onAuthStateChanged } from "firebase/auth";
    import { goto } from "$app/navigation";
  
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
 
  <slot></slot>