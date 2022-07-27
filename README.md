# text-to-speech

html:=


<div class="p-4">
    <div class="wrapper">
        <header>Text To Speech</header>
        <form action="#">
          <div class="row">
            <label>Enter Text</label>
            <textarea></textarea>
          </div>
          <div class="row">
            <label>Select Voice</label>
            <div class="outer">
              <select></select>
            </div>
          </div>
          <button>Convert To Speech</button>
        </form>
        <button >start</button>
      </div>
      
      
      CSS:=
      *{
margin: 0;
padding: 0;
box-sizing: border-box;
font-family: 'Poppins', sans-serif;
}
body{
display: flex;
align-items: center;
justify-content: center;
min-height: 100vh;
background: #5256AD;
}
::selection{
color: #fff;
background: #5256AD;
}
.wrapper{
width: 370px;
padding: 25px 30px;
border-radius: 7px;
background: #fff;
box-shadow: 7px 7px 20px rgba(0,0,0,0.05);
}
.wrapper header{
font-size: 28px;
font-weight: 500;
text-align: center;
}
.wrapper form{
margin: 35px 0 20px;
}
form .row{
display: flex;
margin-bottom: 20px;
flex-direction: column;
}
form .row label{
font-size: 18px;
margin-bottom: 5px;
}
form .row:nth-child(2) label{
font-size: 17px;
}
form :where(textarea, select, button){
outline: none;
width: 100%;
height: 100%;
border: none;
border-radius: 5px;
}
form .row textarea{
resize: none;
height: 110px;
font-size: 15px;
padding: 8px 10px;
border: 1px solid #999;
}
form .row textarea::-webkit-scrollbar{
width: 0px;
}
form .row .outer{
height: 47px;
display: flex;
padding: 0 10px;
align-items: center;
border-radius: 5px;
justify-content: center;
border: 1px solid #999;
}
form .row select{
font-size: 14px;
background: none;
}
form .row select::-webkit-scrollbar{
width: 8px;
}
form .row select::-webkit-scrollbar-track{
background: #fff;
}
form .row select::-webkit-scrollbar-thumb{
background: #888;
border-radius: 8px;
border-right: 2px solid #ffffff;
}
form button{
height: 52px;
color: #fff;
font-size: 17px;
cursor: pointer;
margin-top: 10px;
background: #675AFE;
transition: 0.3s ease;
}
form button:hover{
background: #4534fe;
}

@media(max-width: 400px){
.wrapper{
max-width: 345px;
width: 100%;
}
}


JAVASCRIPT:=
<script>
    const textarea = document.querySelector("textarea"),
voiceList = document.querySelector("select"),
speechBtn = document.querySelector("button");

let synth = speechSynthesis,
isSpeaking = true;

voices();

function voices(){
    for(let voice of synth.getVoices()){
        let selected = voice.name === "Google US English" ? "selected" : "";
        let option = `<option value="${voice.name}" ${selected}>${voice.name} (${voice.lang})</option>`;
        voiceList.insertAdjacentHTML("beforeend", option);
    }
}

synth.addEventListener("voiceschanged", voices);

function textToSpeech(text){
    let utterance = new SpeechSynthesisUtterance(text);
    for(let voice of synth.getVoices()){
        if(voice.name === voiceList.value){
            utterance.voice = voice;
        }
    }
    synth.speak(utterance);
}

speechBtn.addEventListener("click", e =>{
    e.preventDefault();
    if(textarea.value !== ""){
        if(!synth.speaking){
            textToSpeech(textarea.value);
        }
        if(textarea.value.length > 80){
            setInterval(()=>{
                if(!synth.speaking && !isSpeaking){
                    isSpeaking = true;
                    speechBtn.innerText = "Convert To Speech";
                }else{
                }
            }, 500);
            if(isSpeaking){
                synth.resume();
                isSpeaking = false;
                speechBtn.innerText = "Pause Speech";
            }else{
                synth.pause();
                isSpeaking = true;
                speechBtn.innerText = "Resume Speech";
            }
        }else{
            speechBtn.innerText = "Convert To Speech";
        }
    }
});
</script>

