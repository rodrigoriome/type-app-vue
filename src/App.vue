<template>
  <div class="font-sans text-gray-800 px-4">
    <div class="container max-w-lg mx-auto pt-10">
      <h1 class="text-5xl font-black mb-4 text-center">
        TypeApp
      </h1>
      <p class="text-center mb-4">
        Start typing to play
      </p>
      <div class="flex mb-12 justify-center">
        <button
          :class="{ 'bg-gray-200': settings.caseSensitive }"
          class="rounded p-2 border border-gray-300 text-xs mr-4"
          @click.prevent="settings.caseSensitive = !settings.caseSensitive"
        >
          Case sensitive
        </button>
        <button class="rounded p-2 border border-gray-300 text-xs" @click="restartGame()">
          Restart game
        </button>
      </div>
      <div class="relative">
        <input
          v-model="form"
          class="w-full rounded-lg shadow-lg p-4 mb-12 text-lg outline-none"
          :disabled="!gameReady"
          @input="handleInput()"
          @keydown.space.prevent="submit()"
        />
        <div class="absolute top-0 right-0 pt-5 pr-5">
          {{ timer }}
        </div>
      </div>
      <div v-if="!gameOver" class="app-words-container text-4xl text-center text-gray-400">
        <p
          class="app-word"
          v-for="(word, wordIndex) in words"
          :key="wordIndex"
          :class="{
            'text-5xl text-gray-800': wordIndex == index,
            'app-word-hidden': wordIndex < index
          }"
          @ended="alert('foo')"
        >
          {{ word }}
        </p>
      </div>
      <div class="text-center text-xl" v-if="gameOver">
        <span>Words per minute:</span>
        <div class="font-bold">{{ wpm }}</div>
      </div>
    </div>
    <Loading :class="{ 'loading-visible': !gameReady }" />
  </div>
</template>

<script>
import Loading from "./Loading.vue";
import WordsData from "./data/words";

const appDefault = {
  gameReady: false,
  gameStarted: false,
  gameOver: false,
  score: 0,
  index: 0,
  timer: 60,
  interval: null,
  keystrokes: 0,
  form: null
};

export default {
  data() {
    return {
      ...appDefault,
      settings: {
        caseSensitive: true
      },
      words: []
    };
  },
  components: {
    Loading
  },
  computed: {
    wpm() {
      return this.keystrokes / 5;
    }
  },
  methods: {
    handleInput() {
      if (!this.gameStarted) {
        this.startGame();
      }
    },

    startGame() {
      this.gameStarted = true;
      this.interval = setInterval(() => {
        this.timer -= 1;
        if (this.timer <= 0 || this.gameOver === true) {
          this.endGame();
        }
      }, 1000);
    },

    endGame() {
      this.gameOver = true;
      clearInterval(this.interval);
      this.form = null;
    },

    restartGame() {
      this.endGame();
      Object.assign(this.$data, appDefault);
      this.getNewWords();
    },

    /**
     * Eliminates code repetition on the `submit` method
     *
     * @param { string } condition Checks if `settings.caseSensitive` is true or false
     */
    handleSubmit(condition) {
      if (condition) {
        this.keystrokes += this.words[this.index].length;
        this.score++;
      }
    },

    submit() {
      this.handleInput();
      if (!this.gameOver) {
        if (this.settings.caseSensitive) {
          this.handleSubmit(this.form == this.words[this.index]);
        } else {
          this.handleSubmit(this.form.toLowerCase() == this.words[this.index].toLowerCase());
        }
        this.index++;
        this.form = null;
      }
      if (this.index == this.words.length) {
        this.endGame();
      }
    },

    getNewWords() {
      for (let i = 0; i < 120; i++) {
        const random = Math.floor(Math.random() * WordsData.length - 1);
        this.words.push(WordsData[random]);
      }
      this.gameReady = true;
    }
  },

  created() {
    this.getNewWords();
  }
};
</script>

<style lang="scss">
@tailwind base;
@tailwind components;
@tailwind utilities;

* {
  margin: 0;
  padding: 0;
  outline: 0;
  box-sizing: border-box;
}

html,
body,
#root {
  height: 100%;
}

.app-word {
  transition: height 300ms ease-in-out, opacity 300ms ease-in-out, font-size 300ms ease-in-out;
  opacity: 1;
  height: 80px;

  &-hidden {
    opacity: 0;
    height: 0;
    z-index: -1;
  }
}

.app-words-container {
  height: 400px;
  overflow: hidden;
}
</style>
