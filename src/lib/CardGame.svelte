<script>
    import FactCard from "./FactCard.svelte";

    let round = 1;
    const topics = [
        {
            facts: ["I am fun", "I am Super fun", "I am fun and cool"],
        },
        {
            facts: ["I am fun2", "I am Super fun2", "I am fun and cool2"],
        },
        {
            facts: ["I am fun3", "I am Super fun3", "I am fun and coo3l"],
        },
        {
            facts: ["I am fun4", "I am Super 54fun", "I am fun and54 cool"],
        },
    ];
</script>

<p>This is the fun fact game</p>

<fieldset>
    <legend>
        <p>(put theme here?)</p>
    </legend>
    {#each topics.at(round).facts as fact}
        <FactCard {fact} />
    {/each}
</fieldset>

<style>
    fieldset{
        display: grid;
    }
</style>
