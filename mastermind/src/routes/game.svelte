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
     * @param {number} index
     */
function setColor(color, index){
    currentRound[index] = color;
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

<div>
    <button onclick={setColor('red', 0)}>Set Color 1</button>
    <button onclick={setColor('blue', 1)}>Set Color 2</button>
    <button onclick={setColor('green', 2)}>Set Color 3</button>
    <button onclick={setColor('yellow', 3)}>Set Color 4</button>
</div>

<button onclick={submitGuess}>Submit Guess</button>