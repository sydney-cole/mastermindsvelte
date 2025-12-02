<script>

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

let round = $state(0);
let answerKey = $state(['', '', '', '']);
let guessArray =  $state(Array.from({ length: 12 }, () => ['', '', '', '']));
let currentRound =  $state(['', '', '', '']);
let colorIndex = $state(0);

//check lose condition
function gameLost(){
    return round > 11;
}

//provide feedback
function checkFeedback(){
    let rightSpot = 0;
    let rightColor = 0;
    for (let color in guessArray[round]){
        if (answerKey.includes(color)){
            if (guessArray[round].indexOf(color) == answerKey.indexOf(color)) rightSpot++;
            else rightColor++;
        }
    }
    return [rightSpot, rightColor];
}

//set single color
    /**
     * @param {string} color
     */
function setColor(color){
    if (colorIndex > 3) return null;
    currentRound[colorIndex] = color;
    colorIndex++;
    return null;
}

//submit guess
function submitGuess(){
    //error message here?
    if (currentRound.includes('')) return;
    guessArray[round] = currentRound;
    round++;

    let feedback = checkFeedback();
    if (feedback[0] == 4) return 'game won!';
    if (gameLost()) return 'game lost :('
    //add UI stuff here
}

</script>

<div class="gameDiv">
    <div>
        <button class="color-btn red" onclick={setColor('red')}></button>
        <button class="color-btn blue" onclick={setColor('blue')}></button>
        <button class="color-btn green" onclick={setColor('green')}></button>
        <button class="color-btn yellow" onclick={setColor('yellow')}></button>
    </div>

    <button onclick={submitGuess}>Submit Guess</button>
</div>

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
</style>