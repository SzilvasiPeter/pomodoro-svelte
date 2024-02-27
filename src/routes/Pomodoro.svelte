<script>
  import { onMount, onDestroy } from 'svelte';
  import dayjs from 'dayjs';
  import duration from 'dayjs/plugin/duration';

  dayjs.extend(duration);

  let isActive = false;
  let pomodoro = dayjs.duration(25, 'minutes');
  let interval;

  onMount(() => {
    resetTimer();
  });

  onDestroy(() => {
    clearInterval(interval);
  });

  function startTimer() {
    interval = setInterval(() => {
      if (pomodoro <= 0) {
        resetTimer();
      } else {
        pomodoro = pomodoro.subtract(1, 'seconds');
      }
    }, 1000);
  }

  function resetTimer() {
    clearInterval(interval);
    isActive = false;
    pomodoro = dayjs.duration(25, 'minutes');
  }

  function toggleTimer() {
    if (isActive) {
      clearInterval(interval);
      isActive = false;
    } else {
      startTimer();
      isActive = true;
    }
  }
</script>

<div>
  <h1>Pomodoro</h1>
  <p></p>
  <button on:click={toggleTimer}>{isActive ? 'Pause' : 'Start'}</button>
  {#if isActive}
    <button on:click={resetTimer}>Finish</button>
  {/if}
  <p>{pomodoro.format('mm:ss')}</p>
</div>
