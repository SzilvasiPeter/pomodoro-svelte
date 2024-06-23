<script>
  import { onMount, onDestroy } from 'svelte';
  import dayjs from 'dayjs';
  import duration from 'dayjs/plugin/duration';

  dayjs.extend(duration);

  let workTime = 25;
  let shortBreakTime = 5;
  let longBreakTime = 10;

  let dialog;
  let interval;
  let isActive = false;

  let pomodoro = dayjs.duration(workTime, 'minutes');

  onMount(() => {
    dialog = document.getElementById('confirmation-dialog');
    loadSettings();
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
    pomodoro = dayjs.duration(workTime, 'minutes');
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

  function saveSettings() {
    pomodoro = dayjs.duration(workTime, 'minutes');
    localStorage.setItem(
      'pomodoroSettings',
      JSON.stringify({ workTime, shortBreakTime, longBreakTime })
    );
    dialog.close();
  }

  function loadSettings() {
    const savedSettings = localStorage.getItem('pomodoroSettings');
    if (savedSettings) {
      const {
        workTime: savedWorkTime,
        shortBreakTime: savedShortBreakTime,
        longBreakTime: savedLongBreakTime
      } = JSON.parse(savedSettings);
      workTime = savedWorkTime;
      shortBreakTime = savedShortBreakTime;
      longBreakTime = savedLongBreakTime;
    }
  }

  function closeSettings() {
    dialog.close();
  }

  const showDialogClick = () => {
    dialog['showModal']();
  };
</script>

<dialog id="confirmation-dialog">
  <h3>Time</h3>

  <hr />

  <label for="workTime">Pomodoro: {workTime} minutes</label>
  <input type="range" bind:value={workTime} min="1" max="120" step="1" />
  <label for="shortBreakTime">Short Break: {shortBreakTime} minutes</label>
  <input type="range" bind:value={shortBreakTime} min="1" max="120" step="1" />
  <label for="longBreakTime">Long Break: {longBreakTime} minutes</label>
  <input type="range" bind:value={longBreakTime} min="1" max="120" step="1" />

  <br />

  <button type="button" on:click={saveSettings}>Save</button>
  <button type="button" on:click={closeSettings}>Close</button>
</dialog>

<header>
  <button on:click={showDialogClick}>Settings</button>
</header>

<div>
  <p>{pomodoro.format('HH:mm:ss')}</p>
  <button on:click={toggleTimer}>{isActive ? 'Pause' : 'Start'}</button>
  {#if isActive}
    <button on:click={resetTimer}>Finish</button>
  {/if}
</div>
