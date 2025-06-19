<template>
  <main>
    <h1>
      Typr.
    </h1>
    <div id="header">
      <div id="info">{{ info }}</div>
      <div id="buttons">
        <button id="newGameBtn" @click="newGame">New game</button>
      </div>
    </div>
    <div id="game" tabindex="0" @keydown="handleKeydown" @keyup.prevent :class="{ over: isGameOver }">
      <div id="words" :style="{ marginTop: wordsMarginTop + 'px' }">
        <div v-for="(word, wordIndex) in words" :key="wordIndex" :class="['word', { current: wordIndex === currentWordIndex }]">
          <span v-for="(letter, letterIndex) in word.letters" :key="letterIndex" :class="['letter', letter.status]">{{ letter.char }}</span>
        </div>
      </div>
      <div id="cursor" :style="{ top: cursorTop + 'px', left: cursorLeft + 'px' }"></div>
      <div id="focus-error" v-if="!isFocused" @click="focusGame">Click here to focus</div>
    </div>
  </main>
</template>

<script setup>
import { ref, onMounted, onUnmounted, nextTick } from 'vue';

const words = ref([]);
const currentWordIndex = ref(0);
const currentLetterIndex = ref(0);
const info = ref('');
const timer = ref(null);
const gameStart = ref(null);
const gameTime = 30 * 1000;
const isFocused = ref(false);
const isGameOver = ref(false);
const wordsMarginTop = ref(0);
const cursorTop = ref(0);
const cursorLeft = ref(0);

const wordList = 'in one good real one not school set they state high life consider on and not come what also for set point can want as while with of order child about school thing never hold find order each too between program work end you home place around while place problem end begin interest while public or where see time those increase interest be give end think seem small as both another a child same eye you between way do who into again good fact than under very head become real possible some write know however late each that with because that place nation only for each change form consider we would interest with world so order or run more open that large write turn never over open each over change still old take hold need give by consider line only leave while what set up number part form want against great problem can because head so first this here would course become help year first end want both fact public long word down also long for without new turn against the because write seem line interest call not if line thing what work people way may old consider leave hold want life between most place may if go who need fact such program where which end off child down change to from people high during people find to however into small new general it do that could old for last get another hand much eye great no work and with but good there last think can around use like number never since world need what we around part show new come seem while some and since still small these you general which seem will place come order form how about just also they with state late use both early too lead general seem there point take general seem few out like might under if ask while such interest feel word right again how about system such between late want fact up problem stand new say move a lead small however large public out by eye here over so be way use like say people work for since interest so face order school good not most run problem group run she late other problem real form what just high no man do under would to each too end point give number child through so this large see get form also all those course to work during about he plan still so like down he look down where course at who plan way so since come against he all who at world because while so few last these mean take house who old way large no first too now off would in this course present order home public school back own little about he develop of do over help day house stand present another by few come that down last or use say take would each even govern play around back under some line think she even when from do real problem between long as there school do as mean to all on other good may from might call world thing life turn of he look last problem after get show want need thing old other during be again develop come from consider the now number say life interest to system only group world same state school one problem between for turn run at very against eye must go both still all a as so after play eye little be those should out after which these both much house become both school this he real and may mean time by real number other as feel at end ask plan come turn by all head increase he present increase use stand after see order lead than system here ask in of look point little too without each for both but right we come world much own set we right off long those stand go both but under now must real general then before with much those at no of we only back these person plan from run new as own take early just increase only look open follow get that on system the mean plan man over it possible if most late line would first without real hand say turn point small set at in system however to be home show new again come under because about show face child know person large program how over could thing from out world while nation stand part run have look what many system order some one program you great could write day do he any also where child late face eye run still again on by as call high the must by late little mean never another seem to leave because for day against public long number word about after much need open change also'.split(' ');

function randomWord() {
  const randomIndex = Math.floor(Math.random() * wordList.length);
  return wordList[randomIndex];
}

function formatWord(word) {
  return {
    letters: word.split('').map(char => ({ char, status: '', extra: false }))
  };
}

function newGame() {
  words.value = Array.from({ length: 200 }, () => formatWord(randomWord()));
  currentWordIndex.value = 0;
  currentLetterIndex.value = 0;
  info.value = gameTime / 1000 + '';
  clearInterval(timer.value);
  timer.value = null;
  gameStart.value = null;
  isGameOver.value = false;
  wordsMarginTop.value = 0;
  nextTick(updateCursor);
}

function getWpm() {
  const typedWords = words.value.slice(0, currentWordIndex.value + 1);
  const correctWords = typedWords.filter(word => {
    return word.letters.every(letter => letter.status === 'correct');
  });
  return Math.round(correctWords.length / gameTime * 60000);
}

function getAccuracy() {
  const typedWords = words.value.slice(0, currentWordIndex.value + 1);
  if (typedWords.length === 0) return 100;
  
  let totalLetters = 0;
  let correctLetters = 0;
  
  typedWords.forEach(word => {
    word.letters.forEach(letter => {
      if (letter.status === 'correct' || letter.status === 'incorrect') {
        totalLetters++;
        if (letter.status === 'correct') {
          correctLetters++;
        }
      }
    });
  });
  
  return totalLetters === 0 ? 100 : Math.round((correctLetters / totalLetters) * 100);
}

function gameOver() {
  clearInterval(timer.value);
  isGameOver.value = true;
  const wpm = getWpm();
  const accuracy = getAccuracy();
  info.value = `WPM: ${wpm} | Accuracy: ${accuracy}%`;
}

function focusGame() {
  document.getElementById('game')?.focus();
}

function updateCursor() {
  const currentWordEl = document.querySelector('.word.current');
  if (!currentWordEl) return;
  
  const currentLetterEl = currentWordEl.querySelector(`.letter:nth-child(${currentLetterIndex.value + 1})`);
  const nextLetterRect = currentLetterEl?.getBoundingClientRect();
  const nextWordRect = currentWordEl.getBoundingClientRect();
  
  // Get the game container's position for proper relative positioning
  const gameEl = document.getElementById('game');
  const gameRect = gameEl.getBoundingClientRect();

  if (nextLetterRect) {
    cursorTop.value = nextLetterRect.top - gameRect.top + 2;
    cursorLeft.value = nextLetterRect.left - gameRect.left;
  } else if (nextWordRect) {
    cursorTop.value = nextWordRect.top - gameRect.top + 2;
    cursorLeft.value = nextWordRect.right - gameRect.left;
  }

  // Smoother scrolling - check if cursor is getting close to bottom
  if (nextWordRect && nextWordRect.top - gameRect.top > 70) {
    wordsMarginTop.value -= 35;
  }
}

function handleKeydown(ev) {
  if (isGameOver.value) {
    return;
  }
  
  // Prevent default behavior for certain keys
  if (ev.key === 'Backspace' || ev.key === ' ') {
    ev.preventDefault();
  }
  
  const key = ev.key;
  const isLetter = key.length === 1 && key !== ' ';
  const isSpace = key === ' ';
  const isBackspace = key === 'Backspace';

  if (!timer.value && isLetter) {
    gameStart.value = new Date().getTime();
    timer.value = setInterval(() => {
      const currentTime = new Date().getTime();
      const msPassed = currentTime - gameStart.value;
      const sPassed = Math.round(msPassed / 1000);
      const sLeft = Math.round(gameTime / 1000 - sPassed);
      if (sLeft <= 0) {
        gameOver();
        return;
      }
      info.value = sLeft + '';
    }, 1000);
  }

  const currentWord = words.value[currentWordIndex.value];
  if (!currentWord) return;
  
  const currentLetter = currentWord.letters[currentLetterIndex.value];

  if (isLetter) {
    if (currentLetter) {
      currentLetter.status = key === currentLetter.char ? 'correct' : 'incorrect';
      currentLetterIndex.value++;
    } else {
      currentWord.letters.push({ char: key, status: 'incorrect', extra: true });
      currentLetterIndex.value++;
    }
  }

  if (isSpace) {
    if (currentLetterIndex.value !== currentWord.letters.length) {
        currentWord.letters.slice(currentLetterIndex.value).forEach(l => l.status = 'incorrect');
    }
    currentWordIndex.value++;
    currentLetterIndex.value = 0;
  }

  if (isBackspace) {
    if (currentLetterIndex.value > 0) {
      currentLetterIndex.value--;
      const letter = currentWord.letters[currentLetterIndex.value];
      if (letter.extra) {
        currentWord.letters.pop();
      } else {
        letter.status = '';
      }
    } else if (currentWordIndex.value > 0) {
      currentWordIndex.value--;
      const prevWord = words.value[currentWordIndex.value];
      // Clear any extra letters when going back to previous word
      prevWord.letters = prevWord.letters.filter(letter => !letter.extra);
      currentLetterIndex.value = prevWord.letters.length;
    }
  }

  nextTick(updateCursor);
}

onMounted(() => {
  newGame();
  const gameEl = document.getElementById('game');
  gameEl.addEventListener('focus', () => isFocused.value = true);
  gameEl.addEventListener('blur', () => isFocused.value = false);
  gameEl.focus();
});

onUnmounted(() => {
  if (timer.value) {
    clearInterval(timer.value);
  }
});
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;700&family=Roboto+Mono&display=swap');
:root{
    --bgColor: #2c2c2c;
    --textPrimary: #f2f2f2;
    --textSecondary: #888;
    --primaryColor: #fd4; /* Changed to yellow */
    --errorColor: #ff5555;
}
body{
    font-family: 'Poppins', sans-serif;
    background-color:var(--bgColor);
    color: var(--textPrimary);
    font-size: 1.4rem;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
}
main{
    max-width: 700px;
    margin: 50px auto;
    padding: 0 20px;
}
h1{
    color: var(--primaryColor);
    margin-bottom: 20px;
    text-align: center;
    font-weight: 700;
    font-size: 3rem;
}
h1 svg{
    display: none;
}
#header{
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin: 20px 0 40px;
}
#info{
    color: var(--primaryColor);
    font-size: 1.8rem;
    font-weight: 500;
}
button{
    background: var(--primaryColor);
    border:0;
    color: var(--textPrimary);
    padding: 10px 25px;
    border-radius: 8px;
    cursor: pointer;
    transition: background-color 0.3s ease;
    font-weight: 500;
}
button:hover {
    background: #e6c33a; /* Darker yellow for hover */
}
div#game{
    line-height:40px;
    height:120px;
    overflow: hidden;
    position: relative;
    border: 2px solid var(--textSecondary);
    border-radius: 8px;
    padding: 10px;
}
div#game:focus{
    outline:0;
    border-color: var(--primaryColor);
}
#words{
    color: var(--textSecondary);
    transition: filter 0.3s ease;
}
#game:focus #words{
    filter: blur(0);
}
#focus-error {
    position: absolute;
    inset: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    background: rgba(0,0,0,0.5);
    color: var(--textPrimary);
    font-size: 1.5rem;
}
#game:focus #focus-error{
    display:none;
}
div.word{
    display: inline-block;
    font-family: 'Roboto Mono', monospace;
    margin: 0 6px;
    padding: 2px 4px;
    border-radius: 4px;
}
.word.current{
    background: rgba(255, 221, 68, 0.15); /* Yellowish background */
}
.letter.correct{
    color: var(--textPrimary);
}
.letter.incorrect{
    color: var(--errorColor);
    text-decoration: underline;
}
@keyframes blink{
    0%, 100% {
        opacity: 1;
    }
    50% {
        opacity: 0;
    }
}
#cursor{
    display:none;
    width: 2px;
    height: 1.8rem;
    background: var(--primaryColor);
    position: absolute;
    animation: blink 1s infinite;
}
#game:focus #cursor{
    display: block;
}
#game.over #cursor {
    display: none;
}
</style>
