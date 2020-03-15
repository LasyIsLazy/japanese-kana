<template>
    <div id="app">
        <div class="clickable" @click="handleClick">Get</div>
        <div class="clickable" @click="handleKanaClick">{{hiragana}}</div>
        <div class="clickable" @click="handleKanaClick">{{katakana}}</div>
        <div v-show="isKanaVisible">{{kana}}</div>
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
    private handleClick() {
        this.isKanaVisible = false
        this.kana = getRandomKana()
    }
    private handleKanaClick() {
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
    margin-top: 60px;
}
.clickable:hover {
    cursor: pointer;
    -webkit-user-select: none;
    -moz-user-select: none;
}
</style>
