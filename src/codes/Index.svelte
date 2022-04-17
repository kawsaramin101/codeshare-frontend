<script>
    import {
        onMount,
        getContext
    } from 'svelte'
    import axios from "axios";
    import Loading from "../shared/Loading.svelte";
    import CodeCard from "../shared/CodeCard.svelte";
    
    const baseURL = getContext("baseURL");

    let codes = [];
    let isLoading = false;
    let error = null;

    onMount(() => {
        axios.get(`${baseURL}codes/`)
        .then(res => {
            codes = res.data;
            isLoading = false;
            error = null;
        })
        .catch(err => {
            error = err;
            isLoading = false;
        })
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