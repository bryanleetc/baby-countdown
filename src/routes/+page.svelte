<script>
  const dictionary = {
    startButtonText: {
      running_true: 'Stop',
      running_false: 'Start'
    }
  }
  const defaultMins = 1;
  const defaultSecs = 0;
  let defaultTimeSet = calculateTimeInSeconds(defaultMins, defaultSecs);

  export let props = {
    mins: defaultMins,
    secs: defaultSecs,
    timerRunning: false,
    startButtonText: dictionary.startButtonText.running_false,
    timeSet: defaultTimeSet,
    timeRemaining: defaultTimeSet,
    shapeWidth: 300,
    globalTimer: {},
  };

  function startClick() {
    props.timerRunning = !props.timerRunning;

    if (props.timerRunning) {
      props.timeSet = calculateTimeInSeconds(props.mins, props.secs);
      timerStart();
    } else {
      timerStop();
    }

    let dictionaryKey = `running_${props.timerRunning}`
    props.startButtonText = dictionary.startButtonText[dictionaryKey];
  }

  function timerStart() {
    let animationElement = document.querySelector('.star-full');
    let height = props.shapeWidth;
    props.timeRemaining = props.timeSet;

    const intervalId = setInterval(function () {
      props.timeRemaining--;
      height = parseFloat(props.timeSet - props.timeRemaining) / parseFloat(props.timeSet) * props.shapeWidth;
      animationElement.style.height = `${height}px`;

      if (props.timeRemaining === 0) {
        props.timerRunning = false;
        props.startButtonText = dictionary.startButtonText.running_false;
        clearInterval(intervalId)
      }
    }, 1000);

    props.globalTimer = intervalId;
  }

  function timerStop() {
    clearInterval(props.globalTimer);
  }

  function calculateTimeInSeconds(mins, secs) {
    return parseInt(mins) * 60 + parseInt(secs);
  }
</script>

<div>
  <p>Time left: {props.timeRemaining}</p>

  <div class="shape-container" style="width:{props.shapeWidth}px; height:{props.shapeWidth}px">
    <div class="star star-empty" style="width:{props.shapeWidth}px; height:{props.shapeWidth}px">
      <div class="star-shape">
        <div class="star-border"></div>
        <div class="star-clip"></div>
      </div>
    </div>

    <div class="star star-full" style="width:{props.shapeWidth}px; height:0">
      <div class="star-shape" style="width:{props.shapeWidth}px; height:{props.shapeWidth}px">
        <div class="star-border"></div>
        <div class="star-clip"></div>
      </div>
    </div>
  </div>
</div>

<div>
  <label for="minutes">Minutes</label>
  <input type="number" name="minutes" placeholder="Minutes" bind:value={props.mins} />
</div>
<div>
  <label for="seconds">Seconds</label>
  <input type="number" name="seconds" placeholder="Seconds" bind:value={props.secs} />
</div>
<div>
  <button on:click={startClick}>{props.startButtonText}</button>
</div>

<svg style="visibility: hidden; position: absolute;" width="0" height="0" xmlns="http://www.w3.org/2000/svg" version="1.1">
  <defs>
        <filter id="round">
            <feGaussianBlur in="SourceGraphic" stdDeviation="5" result="blur" />    
            <feColorMatrix in="blur" mode="matrix" values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 19 -9" result="goo" />
            <feComposite in="SourceGraphic" in2="goo" operator="atop"/>
        </filter>
    </defs>
</svg>

<style>
  .shape-container {
    position: relative;
    margin: 0 auto;
  }
  .star {
    bottom: 0;
    overflow: hidden;
    position: absolute;
  }
  .star-shape {
    bottom: 0;
    filter:url(#round);
    position: absolute;
    width: 100%;
    height: 100%;
  }
  .star-clip {
    background: yellow;
    clip-path: polygon(
      50% 0%,
      61% 35%,
      98% 35%,
      68% 57%,
      79% 91%,
      50% 70%,
      21% 91%,
      32% 57%,
      2% 35%,
      39% 35%
    );
    bottom: 6%;
    left: 6%;
    width: 88%;
    height: 88%;
    position: absolute;
  }
  .star-border {
    background: black;
    clip-path: polygon(
      50% 0%,
      61% 35%,
      98% 35%,
      68% 57%,
      79% 91%,
      50% 70%,
      21% 91%,
      32% 57%,
      2% 35%,
      39% 35%
    );
    width: 100%;
    height: 100%;
    position: absolute;
  }
  .star-empty .star-clip {
    background: white;
  }
</style>