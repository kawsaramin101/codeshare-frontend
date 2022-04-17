<script>
    import {
        onMount,
        getContext
    } from 'svelte';
    import {
        useParams
    } from "svelte-navigator";
    import axios from "axios";
    import Loading from "../shared/Loading.svelte";
    import Reaction from "../shared/Reaction.svelte";
    
    const params = useParams();
    const baseURL = getContext("baseURL");
    let {userDetails} = getContext('authContext');
    
    let code = null;
    let isLoading = false;
    let error = null;

    onMount(() => {
        
        let headers = {
            'Content-Type': 'application/json',
        }
        
        if (userDetails.isAuthenticated) {
            headers['Authorization'] = `Token ${userDetails.token}`
        } 
        
        axios.get(`${baseURL}codes/code/${$params.id}/`, {
            headers: headers
        })
        .then(res => {
            code = res.data;
            isLoading = false;
            error = null;
        })
        .catch(err => {
            error = err;
            isLoading = false;
        })
    })
    
</script>

<div class="card card-shadow">
    {#if error }
        <p>{error}</p>
    {/if }
    {#if isLoading }
        <Loading />
    {/if }
    {#if code }
        <div class="card-content">
            <h4 class="is-size-4 has-text-weight-bold">{code.title}</h4>
            
            <div class="">
                <div class="ql-editor" contenteditable="true" bind:innerHTML={code.body}></div>
            </div>
                
                <div class="reaction-wrapper">
                    <Reaction  code_or_comment="code" code_or_comment_id={code.id} user_reaction={code.request_user_reaction} number_of_reactions={code.number_of_reactions} />
                </div>
                    
        </div>
    {/if }
</div>