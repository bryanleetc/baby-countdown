<script>
  const dictionary = {
    startButtonText: {
      running_true: 'Pause',
      running_false: 'Start'
    }
  }
  const defaultSecs = 5;

  export let props = {
    defaultSecs: defaultSecs,
    timerRunning: false,
    startButtonText: dictionary.startButtonText.running_false,
    timeRemaining: defaultSecs,
  };

  function startClick() {
    props.timerRunning = !props.timerRunning;
    timerStart();

    let dictionaryKey = `running_${props.timerRunning}`
    props.startButtonText = dictionary.startButtonText[dictionaryKey];
  }

  function timerStart() {
    const intervalId = setInterval(function () {
      props.timeRemaining--;

      if (props.timeRemaining === 0) {
        props.timerRunning = !props.timerRunning;
        clearInterval(intervalId)
      }
    }, 1000);
  }
</script>

<h1>Countdown</h1>
<div>
  <p>Time left:</p>
  <p>{props.timeRemaining}</p>
</div>

<div>
  <input type="text" name="minutes" placeholder="Minutes" />
</div>
<div>
  <input type="text" name="seconds" placeholder="Seconds" value="{props.defaultSecs}" />
</div>
<div>
  <button on:click={startClick}>{props.startButtonText}</button>
</div>