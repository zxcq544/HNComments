<script>
    import CommentLook from "./Comment_look.svelte";

    function hide(ev) {
        ev.target.parentElement.style = "display: none";
    }
    export let comments;
    export let comment_id;
</script>

<ul class="comment_body">
    {#each comments.items[comment_id].kids as kid_1_id}
        {#if comments.items[kid_1_id].dead == null && comments.items[kid_1_id].deleted == null}
            <li>
                <!-- <span>by:{comments.items[kid_1_id].by}</span>
                <span>id:{comments.items[kid_1_id].id}</span>
                <span>time:{new Date(comments.items[kid_1_id].time * 1000)}</span><br />
                {@html comments.items[kid_1_id].text}<br />
                <button class="learn-more">reply</button> -->
                <CommentLook
                    by={comments.items[kid_1_id].by}
                    id={comments.items[kid_1_id].id}
                    time={comments.items[kid_1_id].time}
                    text={comments.items[kid_1_id].text}
                />
                {#if comments.items[kid_1_id].kids}
                    <svelte:self {comments} comment_id={kid_1_id} />
                {/if}
            </li>
        {/if}
    {/each}
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <div class="hider" on:click={hide} />
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
