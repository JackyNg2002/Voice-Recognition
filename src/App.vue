<script setup>
import { ref, onMounted } from 'vue'
const transcript = ref('')
const isRecording = ref(false)

const Recognition = window.SpeechRecognition || window.webkitSpeechRecognition
const sr = new Recognition()

onMounted(() => {
	sr.continuous = true
	sr.interimResults = true

	sr.onstart = () => {
		console.log('SR Started')
		isRecording.value = true
	}

	sr.onend = () => {
		console.log('SR Stopped')
		isRecording.value = false
	}

	sr.onresult = (evt) => {
		for (let i = 0; i < evt.results.length; i++) {
			const result = evt.results[i]

			if (result.isFinal) CheckForCommand(result)
		}

		const t = Array.from(evt.results)
			.map(result => result[0])
			.map(result => result.transcript)
			.join('')
		
		transcript.value = t
	}
})



const CheckForCommand = (result) => {
	const t = result[0].transcript.toLowerCase();
	if (t.includes('stop recording') || t.includes('停止錄音')) {
		const button = document.getElementById("micc");
		button.style.color = "#000000";
		button.style.backgroundColor = "#FFFFFF";
		sr.stop()
	} else if (
		t.includes('What is the time') ||
		t.includes('what\'s the time')
	) {
		sr.stop()
		alert(new Date().toLocaleTimeString())
		setTimeout(() => sr.start(), 100)
	}else if(t.includes('Go forward') || t.includes('go forward') || t.includes('Move ahead')
				|| t.includes('move ahead') || t.includes('前進') || t.includes('向前走') || t.includes('往前走') || t.includes('向前移動')){
		sr.stop()
		console.log("Go forward is match")
		forwardButtonClick();
		setTimeout(() => sr.start(), 500)
	}else if(t.includes('向後走') || t.includes('往後走') || t.includes('往後退') || t.includes('向後移動')){
		sr.stop()
		console.log("Go backward is match")
		backwardButtonClick();
		setTimeout(() => sr.start(), 500)
	}else if(t.includes('向左走') || t.includes('Go left') || t.includes('go left')){
		sr.stop()
		console.log("Go left is match")
		leftButtonClick();
		setTimeout(() => sr.start(), 500)
	}else if(t.includes('向右走') || t.includes('Go right') || t.includes('go right')){
		sr.stop()
		console.log("Go right is match")
		rightButtonClick();
		setTimeout(() => sr.start(), 500)
	}
}

const ToggleMic = () => {
	if (isRecording.value) {
		const button = document.getElementById("micc");
		button.style.color = "#000000";
		button.style.backgroundColor = "#FFFFFF";
		sr.stop()
	} else {
		const button = document.getElementById("micc");
		button.style.color = "#FF0000";
		button.style.backgroundColor = "#FFFFFF";
		sr.start()
		
	}
}

const forwardButtonClick = () => {
	console.log('按钮被触发！');
	const button = document.getElementById("forward");
  	button.style.color = "#465682";
  	button.style.backgroundColor = "#878975";
	
}

const backwardButtonClick = () => {
	console.log('按钮被触发！');
	const button = document.getElementById("backward");
  	button.style.color = "#446682";
  	button.style.backgroundColor = "#878999";
	
}

const leftButtonClick = () => {
	console.log('按钮被触发！');
	const button = document.getElementById("left");
  	button.style.color = "#336682";
  	button.style.backgroundColor = "#877999";
	
}

const rightButtonClick = () => {
	console.log('按钮被触发！');
	const button = document.getElementById("right");
  	button.style.color = "#354885";
  	button.style.backgroundColor = "#486358";
	
}
</script>

<template>
	
	<div class="app">
		<button :class="`mic`" @click="ToggleMic" id="micc"><i class="fa fa-microphone" style="font-size:48px;"></i></button>	
		<div class="transcript" v-text="transcript"></div>
		<br/><button id="forward" style="font-size:48px;" @click="handleButtonClick">Go forward</button>
		<button id="backward" style="font-size:48px;">Go backward</button>
		<button id="left" style="font-size:48px;">Go left</button>
		<button id="right" style="font-size:48px;">Go right</button>
	</div>
</template>

<style>
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
	font-family: 'Fira sans', sans-serif;
}

body {
	background: #281936;
	color: #FFF;
}


</style>
