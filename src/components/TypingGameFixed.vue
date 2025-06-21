<template>
  <main>
    <h1>
      Typr.
    </h1>
    <div id="header">
      <div id="info">{{ info }}</div>
      <div id="controls">
        <div id="mode-selector">
          <button 
            @click="setMode('words')"
            :class="{ active: gameMode === 'words' }"
            class="mode-btn"
          >
            Words
          </button>
          <button 
            @click="setMode('code')"
            :class="{ active: gameMode === 'code' }"
            class="mode-btn"
          >
            Code
          </button>
        </div>
        <div id="time-options">
          <button 
            v-for="option in timeOptions" 
            :key="option.value"
            @click="setGameTime(option.value)"
            :class="{ active: selectedTime === option.value }"
            class="time-btn"
          >
            {{ option.label }}
          </button>
        </div>
        <div id="language-selector" v-if="gameMode === 'code'">
          <select @change="setCodeLanguage($event.target.value)" v-model="selectedLanguage" class="language-select">
            <option v-for="lang in codeLanguages" :key="lang.value" :value="lang.value">
              {{ lang.label }}
            </option>
          </select>
        </div>
        <button id="newGameBtn" @click="newGame">New game</button>
      </div>
    </div>
    <div id="game" tabindex="0" @keydown="handleKeydown" @keyup.prevent :class="{ over: isGameOver, 'code-mode': gameMode === 'code' }">
      <div id="words" :style="{ marginTop: wordsMarginTop + 'px' }" :class="{ 'code-content': gameMode === 'code' }">
        <div v-if="gameMode === 'words'" v-for="(word, wordIndex) in words" :key="wordIndex" :class="['word', { current: wordIndex === currentWordIndex }]">
          <span v-for="(letter, letterIndex) in word.letters" :key="letterIndex" :class="['letter', letter.status]">{{ letter.char }}</span>
        </div>
        <div v-else class="code-container">
          <div class="code-info">{{ selectedLanguage.toUpperCase() }}</div>
          <pre class="code-text"><span v-for="(char, charIndex) in codeText" :key="charIndex" :class="['code-char', char.status, { current: charIndex === currentCharIndex }]">{{ char.char === '\n' ? '\n' : char.char }}</span></pre>
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
const selectedTime = ref(30); // Default to 30 seconds
const gameTime = ref(30 * 1000); // Dynamic game time
const isFocused = ref(false);
const isGameOver = ref(false);
const wordsMarginTop = ref(0);
const cursorTop = ref(0);
const cursorLeft = ref(0);

// Game mode state
const gameMode = ref('words'); // 'words' or 'code'
const selectedLanguage = ref('javascript');
const codeText = ref([]);
const currentCharIndex = ref(0);

// Time options for the user to choose from
const timeOptions = [
  { label: '15s', value: 15 },
  { label: '30s', value: 30 },
  { label: '1m', value: 60 },
  { label: '2m', value: 120 },
  { label: '5m', value: 300 }
];

// Code languages
const codeLanguages = [
  { label: 'JavaScript', value: 'javascript' },
  { label: 'Python', value: 'python' },
  { label: 'TypeScript', value: 'typescript' },
  { label: 'Java', value: 'java' },
  { label: 'C++', value: 'cpp' },
  { label: 'React', value: 'react' },
  { label: 'Vue', value: 'vue' }
];

const wordList = 'in one good real one not school set they state high life consider on and not come what also for set point can want as while with of order child about school thing never hold find order each too between program work end you home place around while place problem end begin interest while public or where see time those increase interest be give end think seem small as both another a child same eye you between way do who into again good fact than under very head become real possible some write know however late each that with because that place nation only for each change form consider we would interest with world so order or run more open that large write turn never over open each over change still old take hold need give by consider line only leave while what set up number part form want against great problem can because head so first this here would course become help year first end want both fact public long word down also long for without new turn against the because write seem line interest call not if line thing what work people way may old consider leave hold want life between most place may if go who need fact such program where which end off child down change to from people high during people find to however into small new general it do that could old for last get another hand much eye great no work and with but good there last think can around use like number never since world need what we around part show new come seem while some and since still small these you general which seem will place come order form how about just also they with state late use both early too lead general seem there point take general seem few out like might under if ask while such interest feel word right again how about system such between late want fact up problem stand new say move a lead small however large public out by eye here over so be way use like say people work for since interest so face order school good not most run problem group run she late other problem real form what just high no man do under would to each too end point give number child through so this large see get form also all those course to work during about he plan still so like down he look down where course at who plan way so since come against he all who at world because while so few last these mean take house who old way large no first too now off would in this course present order home public school back own little about he develop of do over help day house stand present another by few come that down last or use say take would each even govern play around back under some line think she even when from do real problem between long as there school do as mean to all on other good may from might call world thing life turn of he look last problem after get show want need thing old other during be again develop come from consider the now number say life interest to system only group world same state school one problem between for turn run at very against eye must go both still all a as so after play eye little be those should out after which these both much house become both school this he real and may mean time by real number other as feel at end ask plan come turn by all head increase he present increase use stand after see order lead than system here ask in of look point little too without each for both but right we come world much own set we right off long those stand go both but under now must real general then before with much those at no of we only back these person plan from run new as own take early just increase only look open follow get that on system the mean plan man over it possible if most late line would first without real hand say turn point small set at in system however to be home show new again come under because about show face child know person large program how over could thing from out world while nation stand part run have look what many system order some one program you great could write day do he any also where child late face eye run still again on by as call high the must by late little mean never another seem to leave because for day against public long number word about after much need open change also'.split(' ');

// Code snippets for different languages
const codeSnippets = {
  javascript: [
    `function fibonacci(n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

const result = fibonacci(10);
console.log(result);`,
    `const users = [
  { id: 1, name: 'Alice', age: 25 },
  { id: 2, name: 'Bob', age: 30 },
  { id: 3, name: 'Charlie', age: 35 }
];

const adults = users.filter(user => user.age >= 18)
  .map(user => user.name)
  .join(', ');`,
    `async function fetchData(url) {
  try {
    const response = await fetch(url);
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Error:', error);
    throw error;
  }
}`
  ],
  python: [
    `def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[len(arr) // 2]
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    return quick_sort(left) + middle + quick_sort(right)`,
    `class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def greet(self):
        return f"Hello, I'm {self.name} and I'm {self.age} years old"
    
    def is_adult(self):
        return self.age >= 18`,
    `import requests
from datetime import datetime

def get_weather(city):
    api_key = "your_api_key"
    url = f"http://api.openweathermap.org/data/2.5/weather"
    params = {"q": city, "appid": api_key}
    
    response = requests.get(url, params=params)
    return response.json()`
  ],
  typescript: [
    `interface User {
  id: number;
  name: string;
  email: string;
  isActive: boolean;
}

function processUsers(users: User[]): User[] {
  return users.filter(user => user.isActive)
              .sort((a, b) => a.name.localeCompare(b.name));
}`,
    `type ApiResponse&lt;T&gt; = {
  data: T;
  status: 'success' | 'error';
  message?: string;
}

async function fetchApi&lt;T&gt;(url: string): Promise&lt;ApiResponse&lt;T&gt;&gt; {
  const response = await fetch(url);
  return await response.json();
}`,
    `enum Status {
  PENDING = 'pending',
  APPROVED = 'approved',
  REJECTED = 'rejected'
}

class Task {
  constructor(
    public id: string,
    public title: string,
    public status: Status = Status.PENDING
  ) {}
}`
  ],
  react: [
    `import React, { useState, useEffect } from 'react';

const TodoList = () =&gt; {
  const [todos, setTodos] = useState([]);
  const [input, setInput] = useState('');

  const addTodo = () =&gt; {
    if (input.trim()) {
      setTodos([...todos, { id: Date.now(), text: input, done: false }]);
      setInput('');
    }
  };

  return (
    &lt;div&gt;
      &lt;input value={input} onChange={(e) =&gt; setInput(e.target.value)} /&gt;
      &lt;button onClick={addTodo}&gt;Add&lt;/button&gt;
    &lt;/div&gt;
  );
};`,
    `const useCounter = (initialValue = 0) =&gt; {
  const [count, setCount] = useState(initialValue);

  const increment = useCallback(() =&gt; setCount(c =&gt; c + 1), []);
  const decrement = useCallback(() =&gt; setCount(c =&gt; c - 1), []);
  const reset = useCallback(() =&gt; setCount(initialValue), [initialValue]);

  return { count, increment, decrement, reset };
};`
  ],
  vue: [
    `&lt;template&gt;
  &lt;div class="counter"&gt;
    &lt;h2&gt;Count: {{ count }}&lt;/h2&gt;
    &lt;button @click="increment"&gt;+&lt;/button&gt;
    &lt;button @click="decrement"&gt;-&lt;/button&gt;
  &lt;/div&gt;
&lt;/template&gt;

&lt;script setup&gt;
import { ref } from 'vue';

const count = ref(0);
const increment = () =&gt; count.value++;
const decrement = () =&gt; count.value--;
&lt;/script&gt;`,
    `&lt;script setup&gt;
import { computed, reactive } from 'vue';

const state = reactive({
  items: [],
  filter: 'all'
});

const filteredItems = computed(() =&gt; {
  return state.items.filter(item =&gt; {
    if (state.filter === 'completed') return item.done;
    if (state.filter === 'active') return !item.done;
    return true;
  });
});
&lt;/script&gt;`
  ],
  java: [
    `public class BinarySearch {
    public static int search(int[] arr, int target) {
        int left = 0, right = arr.length - 1;
        
        while (left &lt;= right) {
            int mid = left + (right - left) / 2;
            
            if (arr[mid] == target) return mid;
            if (arr[mid] &lt; target) left = mid + 1;
            else right = mid - 1;
        }
        
        return -1;
    }
}`,
    `import java.util.*;

public class StudentManager {
    private List&lt;Student&gt; students;
    
    public StudentManager() {
        this.students = new ArrayList&lt;&gt;();
    }
    
    public void addStudent(String name, int grade) {
        students.add(new Student(name, grade));
    }
    
    public double getAverageGrade() {
        return students.stream()
                      .mapToInt(Student::getGrade)
                      .average()
                      .orElse(0.0);
    }
}`
  ],
  cpp: [
    `#include &lt;iostream&gt;
#include &lt;vector&gt;
#include &lt;algorithm&gt;

class Solution {
public:
    std::vector&lt;int&gt; twoSum(std::vector&lt;int&gt;&amp; nums, int target) {
        std::unordered_map&lt;int, int&gt; map;
        
        for (int i = 0; i &lt; nums.size(); i++) {
            int complement = target - nums[i];
            if (map.find(complement) != map.end()) {
                return {map[complement], i};
            }
            map[nums[i]] = i;
        }
        
        return {};
    }
};`,
    `template&lt;typename T&gt;
class LinkedList {
private:
    struct Node {
        T data;
        Node* next;
        Node(T value) : data(value), next(nullptr) {}
    };
    
    Node* head;
    size_t size;

public:
    LinkedList() : head(nullptr), size(0) {}
    
    void push_front(T value) {
        Node* newNode = new Node(value);
        newNode-&gt;next = head;
        head = newNode;
        size++;
    }
};`
  ]
};

function randomWord() {
  const randomIndex = Math.floor(Math.random() * wordList.length);
  return wordList[randomIndex];
}

function formatWord(word) {
  return {
    letters: word.split('').map(char => ({ char, status: '', extra: false }))
  };
}

function setGameTime(seconds) {
  selectedTime.value = seconds;
  gameTime.value = seconds * 1000;
  if (!timer.value) {
    // Only update info if game hasn't started
    info.value = seconds + '';
  }
}

function setMode(mode) {
  gameMode.value = mode;
  newGame();
}

function setCodeLanguage(language) {
  selectedLanguage.value = language;
  if (gameMode.value === 'code') {
    newGame();
  }
}

function getRandomCodeSnippet(language) {
  const snippets = codeSnippets[language] || codeSnippets.javascript;
  const randomIndex = Math.floor(Math.random() * snippets.length);
  return snippets[randomIndex];
}

function formatCodeText(code) {
  return code.split('').map(char => ({ char, status: '', extra: false }));
}

function newGame() {
  if (gameMode.value === 'words') {
    words.value = Array.from({ length: 200 }, () => formatWord(randomWord()));
    currentWordIndex.value = 0;
    currentLetterIndex.value = 0;
  } else {
    const codeSnippet = getRandomCodeSnippet(selectedLanguage.value);
    codeText.value = formatCodeText(codeSnippet);
    currentCharIndex.value = 0;
  }
  
  info.value = selectedTime.value + '';
  clearInterval(timer.value);
  timer.value = null;
  gameStart.value = null;
  isGameOver.value = false;
  wordsMarginTop.value = 0;
  nextTick(updateCursor);
}

function getWpm() {
  if (gameMode.value === 'words') {
    const typedWords = words.value.slice(0, currentWordIndex.value + 1);
    const correctWords = typedWords.filter(word => {
      return word.letters.every(letter => letter.status === 'correct');
    });
    return Math.round(correctWords.length / gameTime.value * 60000);
  } else {
    // For code mode, calculate based on characters typed
    const typedChars = codeText.value.slice(0, currentCharIndex.value);
    const correctChars = typedChars.filter(char => char.status === 'correct').length;
    // Approximate WPM by dividing correct characters by 5 (average word length)
    return Math.round((correctChars / 5) / gameTime.value * 60000);
  }
}

function getAccuracy() {
  if (gameMode.value === 'words') {
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
  } else {
    // For code mode
    const typedChars = codeText.value.slice(0, currentCharIndex.value);
    if (typedChars.length === 0) return 100;
    
    const correctChars = typedChars.filter(char => char.status === 'correct').length;
    return Math.round((correctChars / typedChars.length) * 100);
  }
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
  if (gameMode.value === 'words') {
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
  } else {
    // Code mode cursor positioning
    const currentCharEl = document.querySelector('.code-char.current');
    if (!currentCharEl) return;
    
    const gameEl = document.getElementById('game');
    const gameRect = gameEl.getBoundingClientRect();
    const charRect = currentCharEl.getBoundingClientRect();
    
    cursorTop.value = charRect.top - gameRect.top + 2;
    cursorLeft.value = charRect.left - gameRect.left;
    
    // Auto-scroll for code mode
    if (charRect.top - gameRect.top > 80) {
      wordsMarginTop.value -= 20;
    }
  }
}

function handleKeydown(ev) {
  if (isGameOver.value) {
    return;
  }
  
  // Prevent default behavior for certain keys
  if (ev.key === 'Backspace' || ev.key === ' ' || ev.key === 'Tab' || ev.key === 'Enter') {
    ev.preventDefault();
  }
  
  const key = ev.key;
  const isLetter = key.length === 1;
  const isBackspace = key === 'Backspace';
  const isEnter = key === 'Enter';
  const isTab = key === 'Tab';

  // Start timer on first keystroke
  if (!timer.value && isLetter) {
    gameStart.value = new Date().getTime();
    timer.value = setInterval(() => {
      const currentTime = new Date().getTime();
      const msPassed = currentTime - gameStart.value;
      const sPassed = Math.round(msPassed / 1000);
      const sLeft = Math.round(gameTime.value / 1000 - sPassed);
      if (sLeft <= 0) {
        gameOver();
        return;
      }
      info.value = sLeft + '';
    }, 1000);
  }

  if (gameMode.value === 'words') {
    handleWordsMode(key, isLetter, isBackspace);
  } else {
    handleCodeMode(key, isLetter, isBackspace, isEnter, isTab);
  }

  nextTick(updateCursor);
}

function handleWordsMode(key, isLetter, isBackspace) {
  const isSpace = key === ' ';
  const currentWord = words.value[currentWordIndex.value];
  if (!currentWord) return;
  
  const currentLetter = currentWord.letters[currentLetterIndex.value];

  if (isLetter && key !== ' ') {
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
}

function handleCodeMode(key, isLetter, isBackspace, isEnter, isTab) {
  if (currentCharIndex.value >= codeText.value.length) return;
  
  const currentChar = codeText.value[currentCharIndex.value];
  let inputKey = key;
  
  // Handle special keys
  if (isEnter) inputKey = '\n';
  if (isTab) inputKey = '  '; // Convert tab to 2 spaces for consistency
  
  if (isLetter || isEnter || isTab) {
    const expectedChar = currentChar.char;
    // For tab handling, check if next chars are spaces
    if (isTab && expectedChar === ' ') {
      // Check if we have double space (tab equivalent)
      const nextChar = codeText.value[currentCharIndex.value + 1];
      if (nextChar && nextChar.char === ' ') {
        currentChar.status = 'correct';
        nextChar.status = 'correct';
        currentCharIndex.value += 2;
      } else {
        currentChar.status = 'incorrect';
        currentCharIndex.value++;
      }
    } else {
      currentChar.status = inputKey === expectedChar ? 'correct' : 'incorrect';
      currentCharIndex.value++;
    }
  }

  if (isBackspace && currentCharIndex.value > 0) {
    currentCharIndex.value--;
    codeText.value[currentCharIndex.value].status = '';
  }
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
    max-width: 900px;
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
    align-items: flex-start;
    margin: 20px 0 40px;
    flex-wrap: wrap;
    gap: 20px;
}
#controls {
    display: flex;
    align-items: center;
    gap: 15px;
    flex-wrap: wrap;
}
#mode-selector {
    display: flex;
    gap: 8px;
}
.mode-btn {
    background: transparent;
    border: 2px solid var(--textSecondary);
    color: var(--textSecondary);
    padding: 8px 16px;
    border-radius: 6px;
    cursor: pointer;
    transition: all 0.3s ease;
    font-weight: 500;
    font-size: 0.9rem;
}
.mode-btn:hover {
    border-color: var(--primaryColor);
    color: var(--primaryColor);
}
.mode-btn.active {
    background: var(--primaryColor);
    border-color: var(--primaryColor);
    color: var(--bgColor);
}
#time-options {
    display: flex;
    gap: 8px;
}
.time-btn {
    background: transparent;
    border: 2px solid var(--textSecondary);
    color: var(--textSecondary);
    padding: 8px 16px;
    border-radius: 6px;
    cursor: pointer;
    transition: all 0.3s ease;
    font-weight: 500;
    font-size: 0.9rem;
}
.time-btn:hover {
    border-color: var(--primaryColor);
    color: var(--primaryColor);
}
.time-btn.active {
    background: var(--primaryColor);
    border-color: var(--primaryColor);
    color: var(--bgColor);
}
#language-selector {
    display: flex;
    align-items: center;
}
.language-select {
    background: var(--bgColor);
    border: 2px solid var(--textSecondary);
    color: var(--textPrimary);
    padding: 8px 12px;
    border-radius: 6px;
    font-size: 0.9rem;
    cursor: pointer;
    transition: border-color 0.3s ease;
}
.language-select:focus {
    outline: none;
    border-color: var(--primaryColor);
}
.language-select option {
    background: var(--bgColor);
    color: var(--textPrimary);
}
#info{
    color: var(--primaryColor);
    font-size: 1.8rem;
    font-weight: 500;
}
button{
    background: var(--primaryColor);
    border:0;
    color: var(--bgColor);
    padding: 10px 25px;
    border-radius: 8px;
    cursor: pointer;
    transition: background-color 0.3s ease;
    font-weight: 500;
}
#newGameBtn:hover {
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
div#game.code-mode {
    height: 200px;
    line-height: 1.5;
}
div#game:focus{
    outline:0;
    border-color: var(--primaryColor);
}
#words{
    color: var(--textSecondary);
    transition: filter 0.3s ease;
    text-align: left;
}
#words.code-content {
    font-family: 'Roboto Mono', monospace;
    font-size: 0.9rem;
    text-align: left;
}
.code-container {
    position: relative;
    text-align: left;
}
.code-info {
    position: absolute;
    top: -8px;
    right: 0;
    background: var(--primaryColor);
    color: var(--bgColor);
    padding: 2px 8px;
    border-radius: 4px;
    font-size: 0.7rem;
    font-weight: 500;
}
.code-text {
    margin: 0;
    white-space: pre-wrap;
    font-family: 'Roboto Mono', monospace;
    text-align: left;
}
.code-char {
    position: relative;
}
.code-char.current {
    background: rgba(255, 221, 68, 0.3);
}
.code-char.correct {
    color: var(--textPrimary);
    background: rgba(0, 255, 0, 0.1);
}
.code-char.incorrect {
    color: var(--errorColor);
    background: rgba(255, 85, 85, 0.2);
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

/* Responsive layout fixes */
@media (max-width: 768px) {
    main {
        max-width: 100%;
        padding: 0 15px;
    }
    
    #header {
        flex-direction: column;
        align-items: center;
        gap: 15px;
    }
    
    #controls {
        flex-direction: column;
        gap: 10px;
    }
    
    #mode-selector,
    #time-options {
        flex-wrap: wrap;
        justify-content: center;
    }
}
</style>
