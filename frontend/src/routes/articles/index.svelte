<script>
    export let posts;
</script>

<style>
    ul, p {
        margin: 0 0 1em 0;
        line-height: 1.5;
    }
    li {
        max-width: 33.333%;
    }
    .main-title {
        font-size: 25px;
    }
</style>

<svelte:head>
    <title>articles</title>
</svelte:head>

<h1>recent posts</h1>

<ul>
    {#each posts as post}
        <li>
            <a class="main-title" rel='prefetch' href='articles/{post.slug}'>
                {post.title}
                <!-- {process.env.BACKEND_URL} -->
                <img src="{process.env.STRAPI_API_URL}{post.media.url}" alt="{post.title}">
            </a>
        </li>
        {#if post.author}
            <p>
                {post.published} by {post.author.username}
            </p>
        {/if}
    {/each}
</ul>

<script context="module">
    export async function preload() {
        const res = await this.fetch('articles/index.json');
        const data = await res.json();

        if (res.status === 200) {
            return data;
        } else {
            this.error(res.status, data.message);
        }
    }
</script>
