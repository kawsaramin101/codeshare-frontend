<script>
    import { getContext } from 'svelte';

    let {userDetails, login, logout } = getContext('authContext');
    const baseURL = getContext("baseURL");
    $: console.log(userDetails, baseURL);
    
    let authDetails = {
        username: "",
        password: "",
    }
    
    function handleLogin() {
        let error = login(authDetails.username, authDetails.password);
        console.log(error)
    }
    
</script>

<form class="box my-5 mx-4" on:submit|preventDefault={handleLogin}>
    <h1 class="has-text-centered subtitle mb-3 fw-normal">Please sign in</h1>

    <div class="field mt-2">
        <label class="label" for="username">Username</label>
        <div class="control">
            <input bind:value={authDetails.username} name="username" class="input" type="text" id="username" placeholder="e.g. John" required>
        </div>
    </div>
    <div class="field my-2">
        <label class="label" for="password">Password</label>
        <div class="control">
            <input bind:value={authDetails.password} name="password" type="password" class="input" id="password" placeholder="Password" required>
        </div>
    </div>
    
    <p id="login-error" class="has-text-danger"></p>

    <div class="buttons is-centered">
        <button class="mt-2 button is-link has-text-centered" type="submit">
            Sign in
            <span id="indicator" class="htmx-indicator spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>
        </button>
    </div>
</form>

<style>
    
</style>

