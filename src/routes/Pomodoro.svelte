<script>
  import { onMount, onDestroy } from 'svelte';
  import dayjs from 'dayjs';
  import duration from 'dayjs/plugin/duration';

  dayjs.extend(duration);

  let interval;
  let isActive = false;
  let minutes = 25;
  $: pomodoro = dayjs.duration(minutes, 'minutes');

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
    pomodoro = dayjs.duration(minutes, 'minutes');
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
  <input bind:value={minutes} disabled={isActive} type="number" />
  <button on:click={toggleTimer}>{isActive ? 'Pause' : 'Start'}</button>
  {#if isActive}
    <button on:click={resetTimer}>Finish</button>
  {/if}
  <p>{pomodoro.format('HH:mm:ss')}</p>
</div>
