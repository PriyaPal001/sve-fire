<script>
    import { goto } from "$app/navigation";
    import { getAuth, signOut } from "firebase/auth";
    import { isLoggedIn } from "../../stores/authStore";
  
    const auth = getAuth();
  
    function logout() {
      signOut(auth)
        .then(() => {
          localStorage.removeItem("uid");
          goto("/login");
        })
        .catch((error) => {
          console.error(error);
        });
    }
  </script>
<div>
    <h1>Hello to the dashboard</h1>
    {#if $isLoggedIn}
    <li class="nav-item">
      <a
        class="nav-link"
        on:click|preventDefault={logout}
        target="_blank"
        href="/">Sign Out</a
      >
    </li>
  {/if}
</div>