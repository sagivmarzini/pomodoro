const pomodoroIntervals = [25, 5, 4, 15]; // [work, short break, number of cycles, long break]

const workMinutes = pomodoroIntervals[0];
const shortBreakMinutes = pomodoroIntervals[1];
const cycles = pomodoroIntervals[2];
const longBreakMinutes = pomodoroIntervals[3];

let currentCycle = 0;
let isWorkInterval = true;

let startingMinutes = workMinutes;
let startingSeconds = startingMinutes * 60;
let totalDuration = startingSeconds * 1000;

const clockOutline = document.getElementById('clock-progress');
const minuteHand = document.getElementById('minute-hand');

const dingSound = new Audio('assets/ding.mp3');

let startTime;
let timerInterval;

function startTimer() {
    startTime = Date.now();
    timerInterval = requestAnimationFrame(updateTimer);
}

function updateTimer() {
    const currentTime = Date.now();
    const elapsedTime = currentTime - startTime;
    const remainingTime = Math.max(totalDuration - elapsedTime, 0);

    updateOutline(remainingTime);
    updateHand(remainingTime);

    if (remainingTime > 0) {
        timerInterval = requestAnimationFrame(updateTimer);
    } else {
        dingSound.play();
        switchIntervals();
    }
}

function switchIntervals() {
    let notificationMessage = '';

    if (isWorkInterval) {
        if (currentCycle < cycles - 1) {
            startingMinutes = shortBreakMinutes;
            currentCycle++;
            notificationMessage = 'Time for a short break!';
        } else {
            startingMinutes = longBreakMinutes;
            currentCycle = 0;
            notificationMessage = 'Time for a long break!';
        }
    } else {
        startingMinutes = workMinutes;
        notificationMessage = 'Time to get back to work!';
    }

    isWorkInterval = !isWorkInterval;
    startingSeconds = startingMinutes * 60;
    totalDuration = startingSeconds * 1000;
    startTimer();

    // Send a notification
    if (Notification.permission === 'granted') {
        sendNotification('Pomodoro Timer', notificationMessage);
    } else if (Notification.permission !== 'denied') {
        Notification.requestPermission().then(permission => {
            if (permission === 'granted') {
                sendNotification('Pomodoro Timer', notificationMessage);
            }
        });
    }
}


function updateOutline(remainingTime) {
    const progressDegrees = (remainingTime / totalDuration) * 360;
    clockOutline.style.background = `conic-gradient(red 0deg, red ${progressDegrees}deg, white ${progressDegrees}deg 360deg)`;
}

function updateHand(remainingTime) {
    const progress = (totalDuration - remainingTime) / totalDuration;
    const minutesDegrees = progress * 360;
    minuteHand.style.transform = `translateX(-50%) rotate(-${minutesDegrees}deg)`;
}

startTimer();
