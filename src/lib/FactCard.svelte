<script>
    export let fact;

    let selected = false;

    function clickedButton() {
        selected = !selected;
    }
</script>

<label>
    <button
        class={selected ? "selected-button" : "notSelected-button"}
        on:click={clickedButton}
    >
        <input type="checkbox" bind:checked={selected} />
        {fact}
    </button>
</label>

<style>
    button{
        padding: auto;
    }
    button:hover {
        box-shadow: 0 6px 10px var(--BoxShaddowColour);
    }
    .selected-button {
        --NotSelectedColour: green;
        --BoxShaddowColour: var(--NotSelectedColour);
        background-color: var(--NotSelectedColour);
    }

    .notSelected-button {
        --NotSelectedColour: red;
        --BoxShaddowColour: var(--NotSelectedColour);
        background-color: var(--NotSelectedColour);
    }
</style>
