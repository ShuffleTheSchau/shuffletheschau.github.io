<script lang="ts">
    import ArticleList from "./lib/ArticleList.svelte";
    import Button from "./lib/Button.svelte";

    let load = false;

    function handleClick() {
        load = true;
    }
</script>

<main class:loaded={load}>
    {#if load}
        <header>
            <h1>Shuffle the Schau!</h1>
        </header>
        <ArticleList></ArticleList>
    {:else}
        <div class="buttonContainer">
            <Button on:click={handleClick} />
        </div>
    {/if}
</main>
<footer>
    <p>
        Shuffle the Schau ist ein Kunstprojekt nach einer Idee von <a
            href="https://cultur.social/@marcuwekling/113317290193122152"
            >Marc-Uwe Kling und dem Känguru</a
        >, umgesetzt von einem bekloppten Studenten und steht in keiner
        Beziehung mit der Tagesschau oder der ARD. Die Daten kommen von der
        öffentlichen API https://tagesschau.api.bund.dev/ und werden live im
        Browser verarbeitet, es findet also keine Neuveröffentlichung statt.
        Manche Ergebnisse können ohne Kontext makaber oder unpassend sein, das
        ist leider nicht zu vermeiden, da alles zufällig generiert wird.
    </p>
</footer>

<style lang="scss">
    .buttonContainer {
        display: flex;
        align-items: center;
        justify-content: center;
        width: 100%;
        min-height: 80vh;
    }

    header {
        display: flex;

        background-color: #1c2841;

        margin-bottom: 2em;

        h1 {
            padding: 0.5em;
        }
    }

    footer {
        margin-block: 1em;
        padding: 1em;

        font-size: 1.5rem;

        a {
            text-decoration: underline;
            color: lightblue;
        }
    }
</style>
