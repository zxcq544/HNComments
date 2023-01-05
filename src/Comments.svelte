<script>
    import { onMount } from "svelte";
    import CommentLook from "./Comment_look.svelte";

    export let comments;
    export let comment_id;
    let child_class;
    onMount(async () => {
        if (comments[comment_id].parent) {
            child_class = "comment_body";
        }
    });
    function show_answers(comment_id_to_show_answers) {
        comments[comment_id_to_show_answers].show_answers = !comments[comment_id_to_show_answers].show_answers;
    }
</script>

<ul class={child_class}>
    {#each comments[comment_id].kids as kid_1_id}
        {#if comments[kid_1_id].dead == null && comments[kid_1_id].deleted == null}
            {#if comments[comment_id].show_answers == null || comments[comment_id].show_answers == true}
                <li>
                    <!-- <span>by:{comments[kid_1_id].by}</span>
                <span>id:{comments[kid_1_id].id}</span>
                <span>time:{new Date(comments[kid_1_id].time * 1000)}</span><br />
                {@html comments[kid_1_id].text}<br />
                <button class="learn-more">reply</button> -->
                    <CommentLook
                        by={comments[kid_1_id].by}
                        id={comments[kid_1_id].id}
                        time={comments[kid_1_id].time}
                        text={comments[kid_1_id].text}
                    />
                    {#if comments[kid_1_id].kids}
                        <button on:click={() => show_answers(kid_1_id)}>
                            {#if comments[kid_1_id].show_answers == false}
                                Show
                            {:else}
                                Hide
                            {/if} answers
                            {comments[kid_1_id].number_of_kids}
                        </button>
                    {/if}

                    {#if comments[kid_1_id].kids}
                        <svelte:self bind:comments comment_id={kid_1_id} />
                    {/if}
                </li>
            {/if}
        {/if}
    {/each}

    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <div class="hider" on:click={() => show_answers(comment_id)} />
</ul>

<style>
    .comment_body {
        border-left: 2px dotted rgba(170, 110, 181, 0.7);
    }
    .hider {
        background-color: azure;
        display: block;
        position: absolute;
        width: 25px;
        height: 100%;
        left: -14px;
        top: 0;
        cursor: pointer;
        opacity: 0%;
    }
    ul {
        position: relative;
        list-style: none;
        margin-bottom: 20px;
    }
    li {
        /* border: 1px solid lightblue; */
        margin-top: 25px;
        margin-bottom: 20px;
    }
</style>
