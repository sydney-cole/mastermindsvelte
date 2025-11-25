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


//start
//check win condition
//check lose condition
//submit guess
//set single color
//provide feedback
//check if single color is correct


</script>