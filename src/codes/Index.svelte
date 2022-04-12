<script>
    import {
        onMount,
        getContext
    } from 'svelte'
    import axios from "axios";
    import Loading from "../shared/Loading.svelte";
    import CodeCard from "../shared/CodeCard.svelte";
    
    const baseURL = getContext("baseURL");

    let codes = [{
        "id": 1,
        "creator": 1,
        "creator_username": "admin",
        "title": "My code 001",
        "body_plain": "Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the",
        "created": "2022-04-07T20:43:36.999879Z"
    }];
    let isLoading = false;
    let error = null;

    onMount(() => {
        /*
        axios.get(`${baseURL}codes/`)
        .then(res => {
            codes = res.data;
            isLoading = false;
            error = null;
        })
        .catch(err => {
            error = err;
            isLoading = false;
        })*/
    })

</script>

<div class="m-4">
    {#if isLoading }
    <div class="is-flex is-justify-content-center">
        <Loading />
    </div>
    {/if }
    {#if error }
    <div>
        {error}
    </div>
    {/if }
    {#each codes as code (code.id) }
    <CodeCard {code} />
    {/each }
</div>