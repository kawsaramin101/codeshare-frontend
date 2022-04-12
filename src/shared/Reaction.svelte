<script>
    import { getContext } from 'svelte';
    import axios from 'axios';
    export let height = "26px";
    export let code_or_comment;
    export let code_or_comment_id;
    export let user_reaction;
    export let number_of_reactions;
    
    const baseURL = getContext("baseURL");
    let {userDetails} = getContext('authContext');
    
    
    let reactions = [
        {value:"like", count: number_of_reactions?.like || 0, display: `<img src='/reactions/like.svg' height=${height} width=${height} />`},
        {value:"dislike", count: number_of_reactions?.dislike || 0, display: `<img src='/reactions/dislike.svg' height=${height} width=${height} />`},
        {value:"wow", count: number_of_reactions?.wow || 0, display: `<img src='/reactions/wow.svg' height=${height} width=${height} />`},
        {value:"love", count: number_of_reactions?.love || 0, display: `<img src='/reactions/love.svg' height=${height} width=${height} />`},
        {value:"haha", count: number_of_reactions?.haha || 0, display: `<img src='/reactions/haha.svg' height=${height} width=${height} />`},
        {value:"sad", count: number_of_reactions?.sad || 0, display: `<img src='/reactions/sad.svg' height=${height} width=${height} />`},
        {value:"rocket", count: number_of_reactions?.rocket || 0, display: `<img src='/reactions/rocket.svg' height=${height} width=${height} />`},
        {value:"angry", count: number_of_reactions?.angry || 0, display: `<img src='/reactions/angry.svg' height=${height} width=${height} />`},
    ]
    
    let reacted = user_reaction ? user_reaction.name : null;
    
    const instance = axios.create({
        baseURL: baseURL,
        headers: {'Authorization': `Token ${userDetails.token}`}
    });
    
    function handleClick(value) {
        console.log(value);
        if ( reacted === value ) {
            instance.delete(`codes/reaction-edit-delete/${user_reaction.id}/`)
            .then((res) => {
                console.log(res);
                number_of_reactions = res.data.number_of_reactions;
                reacted = null;
            })
            .catch(err => {
                console.log(err);
            })
            
        }
        else if (reacted !== null && reacted !== value) {
            instance.patch(`codes/reaction-edit-delete/${user_reaction.id}/`, {name: value})
            .then(res => {
                console.log(res);
                reacted = value;
                user_reaction = res.data.reaction;
                number_of_reactions = res.data.number_of_reactions;
            })
            .catch(err => {
                console.log(err);
            })
            
        }
        else if (reacted === null) {
            const body = {
                code: (code_or_comment === "code" ? code_or_comment_id : null),
                comment: (code_or_comment === "comment" ? code_or_comment_id : null),
                name: value
            }
            instance.post("codes/reaction-create/", body)
                .then(res => {
                    reacted = value;
                    console.log(res);
                    user_reaction = res.data.reaction;
                    number_of_reactions = res.data.number_of_reactions;
                })
                .catch(err => {
                    console.log(err);
                })
        }
    }
    
</script>
    

<div class="is-flex">
    {#each reactions as reaction (reaction.value) }
        <div on:click={() => handleClick(reaction.value)} class="emoji">
            <div class:user-selection={reacted===reaction.value} contenteditable="true" bind:innerHTML={reaction.display}></div>
            <span>
                {number_of_reactions? number_of_reactions[reaction.value] : 0}
            </span>
        </div>
    {/each}
</div>

<style>
    .emoji {
        margin: 2px;
        padding: 2px;
    }
    .user-selection {
        border: 1px solid #77d6ee;
        border-radius: 20%;
        background-color: #ffffffae;
    }
    
</style>