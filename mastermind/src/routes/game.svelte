<script>
import { writable, get } from 'svelte/store';

/* 
STARTING CONDITIONS
- start button
- generated array of 4 colors
- set of 12 empty arrays for guessing
- set of 4 colors
- way to user input
- feedback UI

RULES
- Win: match generated array of colors
- Lose: finish 12 guesses with no right answers
- Round ends on submit button
- can edit color choices if current round

- keep track of round #

*/

let round = writable(0);
let answerKey = writable(["", "", "", ""]);
let guessArray = writable(Array.from({ length: 12 }, () => ["", "", "", ""]));
let feedbackArray = writable(Array.from({ length: 12 }, () => ["", "", "", ""]));
let currentRound = writable(["", "", "", ""]);
let colorIndex = writable(0);
let gameState = writable("START"); //START, IN_PROGRESS, WON, LOST

//check lose condition
function checkGameState(){
    if (get(round) > 11) {
        return gameState.set('LOST');
    }
    if (get(feedbackArray)[get(round) - 1].every(color => color === "red")) {
        return gameState.set('WON');
    }
}

//provide feedback
function checkFeedback(){
    const answer = get(answerKey);
    const guess = get(currentRound);
    let rightSpot = 0;
    let rightColor = 0;
    for (let i = 0; i < guess.length; i++) {
        if (guess[i] === answer[i] && guess[i] !== '') rightSpot++;
        else if (answer.includes(guess[i]) && guess[i] !== '') rightColor++;
    }
    let feedback = [];
    for (let i = 0; i < rightSpot; i++) {
        feedback.push("red");
    }
    for (let i = 0; i < rightColor; i++) {
        feedback.push("white");
    }
    return feedback;
}

//set single color
    /**
     * @param {string} color
     */
function setColor(color) {
    const idx = get(colorIndex);
    const roundArr = get(currentRound);
    if (idx > 3) return null;
    const updatedRound = [...roundArr];
    updatedRound[idx] = color;
    currentRound.set(updatedRound);
    colorIndex.set(idx + 1);
    return null;
}

function submitGuess(){
    const roundVal = get(round);
    const curr = get(currentRound);
    const guesses = get(guessArray);
    const feedback = get(feedbackArray);
    const colorIdx = get(colorIndex);
    //error message here?
    if (colorIdx < 3) return;
    const updatedGuesses = guesses.map((guess, i) => i === roundVal ? [...curr] : guess);
    guessArray.set(updatedGuesses);

    const updatedFeedback = feedback.map((fb, i) => i === roundVal ? checkFeedback() : fb);
    feedbackArray.set(updatedFeedback);

    round.set(roundVal + 1);
    checkGameState();

    currentRound.set(["", "", "", ""]);
    colorIndex.set(0);
    //add UI stuff here
}

//start game
function start(){
    gameState.set('IN_PROGRESS');
    const colors = ["red", "blue", "green", "yellow"];
    round.set(0);
    guessArray.set(Array.from({ length: 12 }, () => ["", "", "", ""]));
    feedbackArray.set(Array.from({ length: 12 }, () => ["", "", "", ""]));
    currentRound.set(["", "", "", ""]);
    colorIndex.set(0);
    answerKey.set(Array.from({ length: 4 }, () => colors[Math.floor(Math.random() * colors.length)]));
}

</script>

<button on:click={start}>Start Game</button>

<div>{#if $gameState !== 'START'}
    <div>
        <ul>
        {#each Array($round) as _, i}
        <li>
            {#each $guessArray[i] as selectedColor}
                <button class="color-btn {selectedColor}"></button>
            {/each}
            {#each $feedbackArray[i] as selectedFeedback}
                <button class="color-btn {selectedFeedback} tiny"></button>
            {/each}
        </li>
        {/each}
        <li>{#each $currentRound as selectedColor}
                <button class="color-btn {selectedColor}"></button>
            {/each}
        </li>
        </ul>
    </div>
    <div>
        <button class="color-btn red" on:click={() => setColor("red")}></button>
        <button class="color-btn blue" on:click={() => setColor("blue")}></button>
        <button class="color-btn green" on:click={() => setColor("green")}></button>
        <button class="color-btn yellow" on:click={() => setColor("yellow")}></button>
    </div>
    {#if $gameState === 'IN_PROGRESS'}
    <button on:click={submitGuess}>Submit Guess</button>
    {/if}
{/if}</div>

<p>{#if $gameState === 'WON'}You won!{/if}</p>
<p>{#if $gameState === 'LOST'}You lost! The answer was 
    {#each $answerKey as answerColor}
        <button class="color-btn {answerColor}"></button>
    {/each}
{/if}</p>

<style>
.color-btn {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    border: 2px solid black;
    margin: 0 8px;
    cursor: pointer;
    display: inline-block;
}
.color-btn.red { background: red; }
.color-btn.blue { background: blue; }
.color-btn.green { background: green; }
.color-btn.yellow { background: yellow; }
.color-btn.white { background: white; }

.color-btn.tiny {width: 5px; height: 5px; margin: 0 2px;}
</style>