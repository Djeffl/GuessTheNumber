<script lang="ts">
  let randomNumber = Math.floor(Math.random() * 20 + 1);
  let value: number | null;
  let text = "";
  $: resultText = isFinished ? `The number was ${randomNumber}` : "";
  let isFinished = false;
  let loseWinText = "";

  let retries = 5;

  const reset = () => {
    retries = 5;
    value = null;
    isFinished = false;
    randomNumber = Math.floor(Math.random() * 20 + 1);
    text = "";
    loseWinText = "";
  };

  const keydown = (e) => {
    if (e.key == "Enter") {
      retries--;
      text =
        Math.abs(randomNumber - value) == 0
          ? `Match!`
          : Math.abs(randomNumber - value) <= 3
          ? "Hot"
          : Math.abs(randomNumber - value) <= 5
          ? "Warm"
          : Math.abs(randomNumber - value) <= 8
          ? "Warm"
          : "Cold";
      if (Math.abs(randomNumber - value) == 0) {
        isFinished = true;
      }
      if (retries == 0) {
        isFinished = true;
      }
      loseWinText =
        retries == 0 && randomNumber - value != 0
          ? "You lose!"
          : randomNumber - value == 0
          ? "You win!"
          : "";

      value = null;
    }
  };
</script>

<h1>Hot & Cold</h1>
<p>The number is between 1 & 20.</p>
<p>Press enter to submit your attempt.</p>

<p>Attempts left: {retries}</p>
<input bind:value on:keydown={keydown} disabled={isFinished} />

<button hidden={!isFinished} on:click={reset}>Play Again!</button>

<p>{loseWinText}</p>
<p>{text}</p>
<p>{resultText}</p>
