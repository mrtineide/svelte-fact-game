<script>
    const topics = [
        {
            facts: [
                "Liker å sitte på første rad i kinosalen",
                "Går på kino minst to ganger i måneden",
                "Eneste gangen jeg skulket en (tysk)time var for å dra på kino midt på dagen",
            ],
            truths: [false, false, true],
            theme: "Kino",
        },
        {
            facts: [
                "Jeg så Game of Thrones sesong 7 før jeg så sesongene 1-6 på 6 dager",
                "Syns at Sesong 8 av Game of Thrones var en god slutt",
                "Meldte meg som statist til Game of Thrones, ble sendt tilbud men de trakk det etter 2 dager",
            ],
            truths: [true, false, false],
            theme: "Game of thrones",
        },
        {
            facts: [
                "Vokste opp med 12 høner og 1 hane bak huset",
                "Slår gress med ljå når det er for høyt for gressklipper hver sommer",
                "Klipper ull fra villsauen til min far sin onkel hver vår",
            ],
            truths: [false, true, false],
            theme: "Gårdsbruk",
        },
        {
            facts: [
                "Mistet mobilen min i utenfor løypen i Myrkdalen og fant den igjen i løssnøen på neste tur ned bakken",
                "Vant 10 000 NOK på 25kr flaxlodd når jeg var 16 år",
                "Falt ned fra en stige på en plen med hode midt mellom to forskjellige planker med spiker pekende slik at jeg kunne blitt blind eller verre 😐",
            ],
            truths: [true, false, false],
            theme: "Fetter Anton flaks",
        },
        {
            facts: ["Sant ✔ ", "Usant ❌"], // Test for mindre en 3 fakta
            truths: [true, false],
            theme: "Jeg var 24 år når jeg lærte at lettkokte havregryn ikke var kokt lett på forhånd.",
        },
        {
            facts: [
                "Har spist antilope når jeg var i Kenya",
                "Har spist alligator når jeg var i Florida",
                "Har spist elg når jeg var i Norge",
            ],
            truths: [false, false, true],
            theme: "Eksotisk kjøtteter",
        },
        {
            facts: [
                "Star Wars episode Ⅰ-Ⅲ er de beste filmene",
                "Star Wars episode Ⅳ-Ⅵ er de beste filmene",
                "Star Wars episode Ⅵ-Ⅸ er de beste filmene",
            ],
            truths: [true, false, false], // 😏
            theme: "Star wars 🌑",
        },
        {
            facts: [
                "Har ikke en Instagram konto",
                "Har en ikke Facebook konto",
                // "Har en ikke Onlyfans konto", // To spicy 🌶
                "Har ikke en Twitter konto",
            ],
            truths: [true, false, false],
            theme: "Sosiale medier",
        },
    ];

    let round = 0;
    let score = 0;
    let checkedAnswers = false;

    // function name referance => https://www.youtube.com/watch?v=Z2o4X-WdAZo
    const nextRoundINGAMEHOW = () => {
        checkedAnswers = false;

        // loop over inputs and then set the checked to false
        myInputs = myInputs.filter((input) => input !== null);
        myInputs.forEach((input) => (input.checked = false));
        selectedButton = selectedButton.map(() => false); // Makes every button red/not selected

        if (round >= topics.length - 1) {
            // loop around and reset score
            // #TODO add endgame summary
            round = 0;
            score = 0;
            progressBar.set(0);
        } else {
            round += 1;
            progressBar.set(round);
        }
    };

    /**
         * This function is triggerd by the button that says "Check answers"
         *
         * 1. Find the selected inputs ✅
         * 2. Check against the Truth table ✅
         * 3. Change the colours to indicate what was true and false
         * 4. Update score counter. ✅
                  
         */
    //let inputNR = -1;
    const checkFacts = (e) => {
        const inputNR = myInputs.findIndex((input) => input.checked);
        // console.log("ionputnr " + inputNR);
        if (inputNR == -1) {
            // No input/button/fact is checked
            alert("Pick a option first!");
            return;
        }
        // Flip to next button
        checkedAnswers = true;

        const truthTable = topics[round].truths;

        if (truthTable[inputNR]) {
            score += 1;
        }
    };

    let myInputs = [];
    function buttonPressed(e) {
        // #TODO There is a unintended effect when you click the label it bubbles the event to the input. This gives the button two click events, one for the input and one for itself
        // console.log("The buttonPressed is called"); // just see here, its printed twice for click on the label. Maybe not a big deal.
        // console.log(e.currentTarget); // Use e.currentTarget as it is the button that has the value and event handler,  and not the event.target hhat may be the input or label element when it is clicked
        const index = e.currentTarget.value;
        if (!myInputs[index].checked) {
            // console.log(selectedButton)
            selectedButton = selectedButton.map(() => false); // Makes every button red/not selected
            selectedButton[index] = true; // Makes the button blue

            myInputs[index].checked = true;
            // console.log(selectedButton)
        }
    }

    $: nrOfOptions = topics[round].facts.length;
    $: selectedButton = new Array(nrOfOptions).fill(false);
    import { tweened } from "svelte/motion";
    import { cubicOut } from "svelte/easing";
    const progressBar = tweened(round, {
        duration: 400,
        easing: cubicOut,
    });
</script>

<p>This is the fun fact game, guess the true fact!</p>
<p>Score is so far {score} out of round {round + 1}</p>
<progress max={topics.length} value={$progressBar} />
<fieldset>
    <legend>
        <p>Tema: {topics.at(round).theme}</p>
    </legend>
    {#each topics.at(round).facts as fact, index}
        <button
            class={selectedButton[index] ? "selected-fact" : "notSelected-fact"}
            on:click={buttonPressed}
            value={index}
        >
            <label for="inputnr{index}">
                <input
                    bind:this={myInputs[index]}
                    type="radio"
                    name="facts"
                    id="inputnr{index}"
                />
                {fact}
            </label>
        </button>
    {/each}
</fieldset>

{#if !checkedAnswers}
    <button
        disabled={myInputs.findIndex(async (input) => input.checked) == -1
            ? true // #BUG #TODO Does not make it disabled
            : false}
        on:click={checkFacts}>Check your answers</button
    >
{:else}
    <button on:click={nextRoundINGAMEHOW}>Click for next round</button>
{/if}

<!-- The selected boxes are the ones I want to submit too check if correct -->

<style>
    p {
        color: var(--text);
    }

    fieldset {
        display: grid;
    }
    button {
        padding: auto;
        width: 100%;
    }
    input {
        background: transparent;
    }

    input:hover,
    input:focus {
        box-shadow: 3px 3px 4px cyan;
    }
    input:indeterminate {
        background: lime;
    }

    button:focus,
    button:hover {
        box-shadow: 0 6px 10px var(--BoxShadowColour);
    }
    .selected-fact {
        --SelectedColour: #0bf976;
        --BoxShadowColour: var(--SelectedColour);
        background-color: var(--SelectedColour);
        color:rgb(45,43,1);
    }

    .notSelected-fact {
        --NotSelectedColour: #bec2fd;
        --BoxShadowColour: var(--NotSelectedColour);
        background-color: var(--NotSelectedColour);
        color:rgb(45,43,1);
    }
</style>
