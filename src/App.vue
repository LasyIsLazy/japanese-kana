<template>
    <div id="app">
        <div class="flex-container">
            <div class="clickable btn" @click="handleNext" data-title="下一个"></div>
            <div class="clickable btn" @click="handleAnswer" data-title="看答案"></div>
        </div>
        <div class="card">
            <div class="title">平假名：</div>
            <div class="text">{{hiragana}}</div>
        </div>
        <div class="card">
            片假名：
            <div class="text">{{katakana}}</div>
        </div>
        <div class="answer card">
            <div class="title">答案：</div>
            <div class="text" v-show="isKanaVisible">{{kana}}</div>
        </div>
    </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import HelloWorld from './components/HelloWorld.vue'
import { Hiragana, Katakana } from './kana'

const firstAlphabets = ['k', 's', 't', 'n', 'h', 'm', 'y', 'r', 'w', 'n']
const lastAlphabets = ['a', 'i', 'u', 'e', 'o']
function getRandomKana(): string {
    const kana =
        firstAlphabets[Math.floor(Math.random() * firstAlphabets.length)] +
        lastAlphabets[Math.floor(Math.random() * lastAlphabets.length)]
    if (Hiragana[kana] && Katakana[kana]) {
        return kana
    }
    return getRandomKana()
}

@Component({
    components: {
        HelloWorld,
    },
})
export default class App extends Vue {
    private kana = ''
    private isKanaVisible = false
    private get hiragana() {
        return Hiragana[this.kana]
    }
    private get katakana() {
        return Katakana[this.kana]
    }
    private handleNext() {
        this.isKanaVisible = false
        this.kana = getRandomKana()
    }
    private handleAnswer() {
        this.isKanaVisible = true
    }
}
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    width: 400px;
    margin: 60px auto 0 auto;
}
.clickable:hover {
    cursor: pointer;
    -webkit-user-select: none;
    -moz-user-select: none;
}
.btn {
    position: relative;
    display: inline-block;
    padding: 1.2em 2em;
    text-decoration: none;
    text-align: center;
    cursor: pointer;
    user-select: none;
    color: white;
}

.btn::before {
    z-index: -1;
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    background: linear-gradient(135deg, #6e8efb, #a777e3);
    border-radius: 4px;
    transition: box-shadow 0.5s ease, transform 0.2s ease;
    will-change: transform;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
    transform: translateY(var(--ty, 0)) rotateX(var(--rx, 0))
        rotateY(var(--ry, 0)) translateZ(var(--tz, -12px));
}

.btn:hover::before {
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
}

.btn::after {
    position: relative;
    display: inline-block;
    content: attr(data-title);
    transition: transform 0.2s ease;
    font-weight: bold;
    letter-spacing: 0.01em;
    will-change: transform;
    transform: translateY(var(--ty, 0)) rotateX(var(--rx, 0))
        rotateY(var(--ry, 0));
}
.flex-container {
    display: flex;
    justify-content: space-around;
    align-items: center;
}
.card {
    margin: 0 auto;
    width: 300px;
    height: 110px;
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.text {
    font-size: 80px;
    background-color: red;
    background-image: linear-gradient(
        45deg,
        transparent 45%,
        hsla(48, 20%, 90%, 1) 45%,
        hsla(48, 20%, 90%, 1) 55%,
        transparent 0
    );
    background-size: 0.05em 0.05em;
    background-clip: text;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    -webkit-text-stroke: 2px #111;
}
.answer > .text {
    background-color: green;
}
</style>
