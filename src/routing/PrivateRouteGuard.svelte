<script>
  import { useNavigate, useLocation } from "svelte-navigator";
  import { getContext } from 'svelte';
  
  let {userDetails, login, logout } = getContext('authContext');
    
  const navigate = useNavigate();
  const location = useLocation();

  $: if (!userDetails.isAuthenticated) {
    navigate("/login", {
      state: { from: $location.pathname },
      replace: true,
    });
  }
</script>

{#if userDetails.isAuthenticated}
  <slot />
{/if}
