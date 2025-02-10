<script lang="ts">
    import sanitizeHtml from "sanitize-html";

    let images: any = [];

    async function fetchData() {
        const res = await fetch("https://www.tagesschau.de/api2u/homepage");

        const data = await res.json();

        console.log(data);

        if (res.ok) {
            // Grab all images
            let newImages: any = [];
            for (let entry of data.news) {
                // Trying to make this as accessible as possible
                let image = {
                    image: entry.teaserImage?.imageVariants["16x9-640"],
                    alt_text: entry.teaserImage?.alttext,
                };

                newImages.push(image);
            }

            images = shuffleArray(newImages);

            console.log(images);

            return data;
        } else {
            throw new Error(data);
        }
    }

    function shuffleArray(array: Array<Object>) {
        if (array.length > 0) {
            let newArray = Array(array.length);

            let i = array.length;

            // Felt cute, might start an infinite loop later
            while (i >= 0) {
                let curr = array[i];
                let newIndex = Math.floor(Math.random() * array.length);

                if (i != newIndex && newArray[newIndex] == null) {
                    newArray[newIndex] = curr;
                    i--;
                }
            }

            return newArray;
        }
    }
</script>

{#await fetchData()}
    Loading!
{:then items}
    <div class="article-list">
        {#each items.news as item, i}
            {#if item.content}
                <div class="article">
                    <div class="article-image">
                        <img src={images[i].image} alt={images[i].alt_text} />
                    </div>

                    <a href={item.detailsweb}>
                        <main>
                            <h2 class="topline">{item.topline}</h2>
                            <h1 class="title">{item.title}</h1>

                            <div class="article-except">
                                {@html sanitizeHtml(item.content[0]?.value, {
                                    allowedTags: ["b", "i", "em", "strong"],
                                })}
                                <span class="link-extend"> mehr </span>
                            </div>
                        </main>
                    </a>
                </div>
            {/if}
        {/each}
    </div>
{:catch error}
    <p style="color: red;">{error.message}</p>
{/await}

<style lang="scss">
    :global(body) {
        background-color: #000e29;
    }

    .article-list {
        display: flex;
        flex-direction: column;

        .article {
            display: grid;
            grid-template-columns: 1fr 2fr;
            gap: 1em;
            margin-block: 1em;
            background-color: #1c2841;

            .article-image {
                img {
                    height: 100%;
                    object-fit: contain;
                }
            }

            .link-extend::before {
                content: "|";
                margin: 0 0.4rem;
            }

            main {
                padding-inline: 2em;
                padding-block: 1em;

                .topline {
                    font-size: 1rem;
                    margin-bottom: 0.2rem;
                    font-weight: 400;
                }

                .title {
                    font-weight: 400;
                    font-size: 1.8rem;
                    line-height: 3.2rem;
                    margin-bottom: 0.5rem;
                }

                .article-excerpt {
                    display: block;
                    font-size: 0.8rem;
                    line-height: 2.4rem;
                }
            }
        }
    }

    @media screen and (max-width: 600px) {
        .article-list {
            .article {
                grid-template-columns: 1fr;
            }
        }
    }
</style>
