<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Free AI Voice Generator</title>
<style>
body{
    font-family: Arial;
    background: #111;
    color: white;
    text-align: center;
    padding: 20px;
}
textarea{
    width: 90%;
    height: 300px;
    font-size: 16px;
    padding: 10px;
}
button{
    padding: 10px 20px;
    font-size: 18px;
    margin: 10px;
    cursor: pointer;
}
select{
    padding: 8px;
    font-size: 16px;
}
</style>
</head>
<body>

<h1>Free AI Voice Generator</h1>

<textarea id="text" placeholder="Paste 25000+ words here..."></textarea>
<br>

<select id="voiceSelect"></select>
<br>

<button onclick="speakText()">▶ Convert to Voice</button>
<button onclick="stopSpeech()">⏹ Stop</button>

<script>
let voices = [];
let speech = window.speechSynthesis;

function loadVoices(){
    voices = speech.getVoices();
    let voiceSelect = document.getElementById("voiceSelect");
    voiceSelect.innerHTML = "";
    voices.forEach((voice, i)=>{
        let option = document.createElement("option");
        option.value = i;
        option.textContent = voice.name + " (" + voice.lang + ")";
        voiceSelect.appendChild(option);
    });
}

speech.onvoiceschanged = loadVoices;

function speakText(){
    let text = document.getElementById("text").value;
    let selectedVoice = voices[document.getElementById("voiceSelect").value];

    let chunks = text.match(/.{1,2000}/g);

    function speakChunk(i){
        if(i >= chunks.length) return;
        let utterance = new SpeechSynthesisUtterance(chunks[i]);
        utterance.voice = selectedVoice;
        utterance.onend = ()=> speakChunk(i+1);
        speech.speak(utterance);
    }

    speakChunk(0);
}

function stopSpeech(){
    speech.cancel();
}
</script>

</body>
</html>
