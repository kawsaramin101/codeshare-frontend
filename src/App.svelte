<script>
    import { writable } from 'svelte/store'
    import { onDestroy, setContext } from 'svelte'
    
    import { Router, Link, Route } from "svelte-navigator";
    import 'bulma/css/bulma.min.css';
    import axios from "axios";
    import Quill from 'quill/core';
    
   // var ColorClass = Quill.import('attributors/class/color');
    //var SizeStyle = Quill.import('attributors/style/size');
  //  Quill.register(ColorClass, true);
   
    
    import PrivateRoute from "./routing/PrivateRoute.svelte";
    import Navbar from "./components/Navbar.svelte";
    import Login from "./users/Login.svelte";
    import Index from "./codes/Index.svelte";
    import CodeDetails from "./codes/CodeDetails.svelte";
    import Reaction from "./shared/Reaction.svelte";
    
    
    const baseURL = "http://127.0.0.1:8000/api/v1/";
    
    let userDetails = writable(JSON.parse(localStorage.getItem("userDetails")) || {
        isAuthenticated: false,
        username: null,
        userID: null,
        token: null,
    });
    
    function login(username, password) {
        let error = null;
        axios.post(`${baseURL}users/token-auth/`, {username,password})
        .then((res) => {
            console.log(res.data)
            userDetails.set({
                isAuthenticated: true,
                username: res.data.username,
                userID: res.data.user_id,
                token: res.data.token,
            })
            localStorage.setItem("userDetails", JSON.stringify($userDetails));
            err = null;
        })
        .catch(err => {
            console.log(err)
            error = err;
        })
        return error;
    }
    
    function logout() {
        userDetails.set({
            isAuthenticated: false,
            username: null,
            userID: null,
            token: null,
        })
        localStorage.setItem("userDetails", JSON.stringify($userDetails));
    }
    
    setContext('authContext', {userDetails: $userDetails, login, logout});
    setContext('baseURL', baseURL);
	
</script>


<Router>
    <main>
        <Navbar />
        <Route path="/">
            <div>
                
            {$userDetails.username}
            <button on:click={logout}>logout</button>
            
            <Reaction />
            
            <Index />
            </div>
        </Route>
        
        <Route path="code/:id">
            <CodeDetails />
        </Route>
        
        <Route path="login">
            <Login />
        </Route>
        
    	
    </main>
</Router>

<style>
	
</style>