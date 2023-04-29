<script>
    import { fade, scale } from "svelte/transition";
    import Card from "./lib/Card.svelte";
    let alarms = [];
    let input, stop;
    let show = true,
        loading = true,
        showalarm = false,
        showAlarmStop = false,
        alarmStopped = false;
    let time = new Date();
    let timeString = time.getHours() + ":" + time.getMinutes();
    function next() {
        show = false;
        setTimeout(() => {
            loading = false;
        }, 1000);
        input.addEventListener("input", () => {
            alarms.push(input.value);
            alarms = alarms;
            let alarms_obj = [];
            for (const alarm of alarms) {
                const time = alarm.split(":");
                alarms_obj.push({
                    hh: parseInt(time[0]),
                    mm: parseInt(time[1]),
                });
            }
            fetch("/", {
                method: "POST", // or 'PUT'
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify(alarms_obj),
            });
        });
    }
    function addAlarm() {
        input.showPicker();
    }
    function checkAlarm() {
        for (const i of alarms) {
            if (timeString === i && alarmStopped == false) {
                showAlarmStop = true;
            }
            console.log(i);
        }
        let time = new Date();
        timeString =
            time.getHours() +
            ":" +
            time.getMinutes().toString().padStart(2, "0");
        stop.addEventListener("click", () => {
            fetch("/stop", {
                method: "POST", // or 'PUT'
            });
            showAlarmStop = false;
            alarmStopped = true;
        });
    }
    setInterval(checkAlarm, 1000);
</script>

<input bind:this={input} type="time" />
{#if show}
    <img
        alt="titan logo"
        src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4NCjwhRE9DVFlQRSBzdmcgUFVCTElDICItLy9XM0MvL0RURCBTVkcgMS4xLy9FTiIgImh0dHA6Ly93d3cudzMub3JnL0dyYXBoaWNzL1NWRy8xLjEvRFREL3N2ZzExLmR0ZCI+DQo8IS0tIENyZWF0b3I6IENvcmVsRFJBVyAyMDE4ICg2NC1CaXQpIC0tPg0KPHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHhtbDpzcGFjZT0icHJlc2VydmUiIHdpZHRoPSI2Ljg4NDQyaW4iIGhlaWdodD0iMS4yNTQ2OGluIiB2ZXJzaW9uPSIxLjEiIHN0eWxlPSJzaGFwZS1yZW5kZXJpbmc6Z2VvbWV0cmljUHJlY2lzaW9uOyB0ZXh0LXJlbmRlcmluZzpnZW9tZXRyaWNQcmVjaXNpb247IGltYWdlLXJlbmRlcmluZzpvcHRpbWl6ZVF1YWxpdHk7IGZpbGwtcnVsZTpldmVub2RkOyBjbGlwLXJ1bGU6ZXZlbm9kZCINCnZpZXdCb3g9IjAgMCA2ODg0LjQyIDEyNTQuNjgiDQogeG1sbnM6eGxpbms9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiPg0KIDxkZWZzPg0KICA8c3R5bGUgdHlwZT0idGV4dC9jc3MiPg0KICAgPCFbQ0RBVEFbDQogICAgLmZpbDAge2ZpbGw6IzM3MzQzNX0NCiAgIF1dPg0KICA8L3N0eWxlPg0KIDwvZGVmcz4NCiA8ZyBpZD0iTGF5ZXJfeDAwMjBfMSI+DQogIDxtZXRhZGF0YSBpZD0iQ29yZWxDb3JwSURfMENvcmVsLUxheWVyIi8+DQogIDxwYXRoIGNsYXNzPSJmaWwwIiBkPSJNMTE5MC4yMSA0MjAuODdjMTA1LjMxLDI1Ni4xOSAtNDcuNTgsNjMwLjU4IC0yOTMuNzQsNzIwLjAxbDAgLTk3My41NyAzOTAuOTEgMC4wMyAxMDMuMzQgLTE2Ny4zNCAtNjYyLjMxIDAgMC4xNyAxMTY4LjM2IDAgNjQuNzMgMCAyMS41OWMzNDMuMSwtMjIuNDQgNjU3LjM1LC0zMDMuODMgNjU3LjM1LC02NjAuNDUgMCwtNTkuMDkgLTcuNjYsLTExNy4wNiAtMjIuMDQsLTE3My4zNmwtMTczLjY3IDB6bS01MjguMyAtNDIwLjg3bDAgMCAtNjYxLjkyIDAgMTAzLjAzIDE2Ny4zNCAzOTAuODMgLTAuMDIgMCA5NzMuNTVjLTI0My43LC04OC41NCAtNDAyLjg1LC00NjkgLTI5Mi44OSwtNzIwLjAxbC0xNzQuNTMgMGMtMTQuMzcsNTYuMyAtMjIuMDMsMTE0LjI3IC0yMi4wMywxNzMuMzYgMCwzNTYuNjIgMzE0LjI0LDYzOC4wMSA2NTcuMzQsNjYwLjQ1bDAgLTIxLjU5IDAgLTY0LjczIDAuMTcgLTExNjguMzZ6bTIzMDEuNjkgMGwwIDAgMCA5Ni4wMiAtMzc0Ljg0IDAgMCAxMDYzLjI1IC0xOTQuNzcgMCAwIC0xMDYzLjI1IC0zNzUuNjggMCAwIC05Ni4wMiA5NDUuMjkgMHptMjk4MS4wNyAwbDAgMCAxMDYuNTMgMCA3MzkuMiA4MjcuNTkgMCAtODI3LjU5IDk0LjAxIDAgMCAxMTU5LjI2IC00My44OSAwIC04MDEuODQgLTkxOC42NSAwIDkyMi43IC05NC4wMiAyLjIyIDAgLTExNjUuNTR6bS0xMDA3LjY5IDcxMC41OGwwIDAgNDExLjMxIDAgLTE5OC45OCAtNDUwIC0yMTIuMzMgNDUwem0yNzguOTUgLTcxMC41OGwwIDAgNTM1Ljg1IDExNjMuODkgLTE5NC4zOSAwIC0xNzMuOTYgLTM3OS44MiAtNDc5LjkxIDAgLTE3OS42OSAzNzkuODIgLTEwMy4yOSAwIDU0OC4wNyAtMTE2My44OSA0Ny4zMyAwem0tNTc2LjA5IDBsMCAwIC05NDUuMyAwIDAgOTYuMDEgMzc1LjY4IDAgMCAxMDYzLjI2IDE5NC43NyAwIDAgLTEwNjMuMjYgMzc0Ljg0IDAgMCAtOTYuMDF6bS0xNDA4LjExIDBsMCAwIDE5NC43OCAwIDAgMTE1OS4yNyAtMTk0Ljc4IDAgMCAtMTE1OS4yN3oiLz4NCiA8L2c+DQo8L3N2Zz4NCg=="
    />
    <button on:click={next} class="normal">Continue</button>
{:else if loading}
    <p class="loader" />
    <p>Pairing...</p>
{:else if !showalarm}
    <div class="icons" transition:fade>
        <button on:click={() => (showalarm = true)} class="icon">
            <svg
                xmlns="http://www.w3.org/2000/svg"
                width="36"
                height="36"
                viewBox="0 0 24 24"
                ><path
                    fill="whitesmoke"
                    d="m22 5.72l-4.6-3.86l-1.29 1.53l4.6 3.86L22 5.72zM7.88 3.39L6.6 1.86L2 5.71l1.29 1.53l4.59-3.85zM12.5 8H11v6l4.75 2.85l.75-1.23l-4-2.37V8zM12 4c-4.97 0-9 4.03-9 9s4.02 9 9 9a9 9 0 0 0 0-18zm0 16c-3.87 0-7-3.13-7-7s3.13-7 7-7s7 3.13 7 7s-3.13 7-7 7z"
                /></svg
            >
        </button>
        <button class="icon">
            <svg
                xmlns="http://www.w3.org/2000/svg"
                width="36"
                height="36"
                viewBox="0 0 24 24"
                ><path
                    fill="whitesmoke"
                    d="M9 1h6v2H9zm10.03 6.39l1.42-1.42c-.43-.51-.9-.99-1.41-1.41l-1.42 1.42A8.962 8.962 0 0 0 12 4a9 9 0 0 0-9 9c0 4.97 4.02 9 9 9a8.994 8.994 0 0 0 7.03-14.61zM13 14h-2V8h2v6z"
                /></svg
            >
        </button>
        <button class="icon">
            <svg
                xmlns="http://www.w3.org/2000/svg"
                width="36"
                height="36"
                viewBox="0 0 24 24"
                ><path
                    fill="whitesmoke"
                    d="m18 22l-.01-6L14 12l3.99-4.01L18 2H6v6l4 4l-4 3.99V22h12zM8 7.5V4h8v3.5l-4 4l-4-4z"
                /></svg
            >
        </button>
    </div>
{:else}
    <main transition:fade>
        <h1 class="top">{timeString}</h1>
        <div class="alarms">
            {#each alarms as alarm}
                <Card time={alarm} />
            {/each}
        </div>
        <button class="normal" on:click={addAlarm}>Add Alarm</button>
        {#if showAlarmStop}
            <button transition:scale bind:this={stop} class="circle bottom"
                ><svg
                    xmlns="http://www.w3.org/2000/svg"
                    width="24"
                    height="24"
                    viewBox="0 0 24 24"
                    ><path
                        fill="currentColor"
                        d="M12 6c3.87 0 7 3.13 7 7c0 .84-.16 1.65-.43 2.4l1.52 1.52c.58-1.19.91-2.51.91-3.92a9 9 0 0 0-9-9c-1.41 0-2.73.33-3.92.91L9.6 6.43C10.35 6.16 11.16 6 12 6zm10-.28l-4.6-3.86l-1.29 1.53l4.6 3.86L22 5.72zM2.92 2.29L1.65 3.57L2.98 4.9l-1.11.93l1.42 1.42l1.11-.94l.8.8A8.964 8.964 0 0 0 3 13c0 4.97 4.02 9 9 9c2.25 0 4.31-.83 5.89-2.2l2.2 2.2l1.27-1.27L3.89 3.27l-.97-.98zm13.55 16.1C15.26 19.39 13.7 20 12 20c-3.87 0-7-3.13-7-7c0-1.7.61-3.26 1.61-4.47l9.86 9.86zM8.02 3.28L6.6 1.86l-.86.71l1.42 1.42l.86-.71z"
                    /></svg
                >
            </button>
        {/if}
    </main>
{/if}

<style>
    input[type="time"] {
        display: none;
        position: absolute;
    }
    .alarms {
        display: flex;
        flex-direction: column;
        gap: 4px;
    }

    .loader {
        width: 48px;
        height: 48px;
        border: 2px solid #373435;
        border-radius: 50%;
        display: inline-block;
        position: relative;
        box-sizing: border-box;
        animation: rotation 1s linear infinite;
    }
    .loader::after {
        content: "";
        box-sizing: border-box;
        position: absolute;
        left: 50%;
        top: 0;
        background: #404040;
        width: 3px;
        height: 24px;
        transform: translateX(-50%);
    }

    button {
        background: #373435;
        color: #f5f5f5;
        border-style: none;
        font-size: 1em;
    }
    .normal {
        border-radius: 9999px;
        padding: 12px 24px;
        margin-top: 2em;
        margin-bottom: 2em;
    }
    .icon {
        border-radius: 4px;
        padding: 14px 16px;
    }
    .circle {
        border-radius: 9999px;
        padding: 14px 16px;
        align-self: center;
    }
    @keyframes rotation {
        0% {
            transform: rotate(0deg);
        }
        100% {
            transform: rotate(360deg);
        }
    }
    img {
        max-width: 320px;
    }
    h1 {
        color: #373435;
    }
    .top {
        position: absolute;
        top: 2em;
        left: 50%;
        transform: translate(-50%, 0);
    }
    .bottom {
        position: absolute;
        bottom: 2em;
        left: 50%;
        transform: translate(-50%, 0);
    }
</style>
