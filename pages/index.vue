<template>
  <div id="app">
    <h1 class="title">NUMERON</h1>
    <div>
      <h1 v-if="clear">CLEAR!!</h1>
      <button v-if="clear" v-on:click="reset">reset</button>
    </div>
    <h3 class="error" v-if="error_message">{{error_message}}</h3>
    <!-- <h2>{{answer_num}}</h2> -->
    <input type="text" v-model="suggest_num">
    <form v-on:submit.prevent>
      <button v-on:click="attack">attack</button>
    </form>
    <h3 class="log">Attack log</h3>
    <li v-for="suggest in suggests_log">{{suggest.item}}: {{suggest.eat}} EAT, {{suggest.bite}} BITE</li>
  </div>
</template>

<script>
import Logo from "~/components/Logo.vue";
import IconLink from "~/components/IconLink.vue";
const init_answer = function() {
  let num = [];
  let tmp = "";
  while (num.length < 4) {
    tmp = Math.floor(Math.random() * 10);
    if (!num.includes(tmp)) num.push(tmp);
  }
  return num.join("");
};
export default {
  components: {
    Logo,
    IconLink
  },
  data() {
    let num = [];
    let tmp = "";
    while (num.length < 4) {
      tmp = Math.floor(Math.random() * 10);
      if (!num.includes(tmp)) num.push(tmp);
    }
    return {
      error_message: "",
      clear: false,
      answer_num: init_answer(),
      suggest_num: "",
      suggests_log: []
    };
  },
  methods: {
    reset: function(event) {
      this.error_message = "";
      this.clear = false;
      this.answer_num = init_answer();
      this.suggest_num = "";
      this.suggests_log = [];
    },
    attack: function(event) {
      if (
        this.suggest_num.length !== new Set(this.suggest_num.split("")).size
      ) {
        this.error_message = "数字が重複しています。";
        return 0;
      } else if (isNaN(Number(this.suggest_num))) {
        this.error_message = "数字以外が含まれています。";
        return 0;
      } else if (this.suggest_num.length > 4) {
        this.error_message = "数字が長すぎます";
        return 0;
      } else {
        this.error_message = "";
      }
      let eat = 0;
      let bite = 0;
      for (let i = 0; i < this.suggest_num.length; i++) {
        if (this.suggest_num[i] === this.answer_num[i]) {
          eat++;
        } else {
          for (let j = 0; j < this.suggest_num.length; j++) {
            if (
              i !== j &&
              this.suggest_num[i] === this.answer_num[j] &&
              this.suggest_num[j] !== this.answer_num[j]
            ) {
              bite++;
            }
          }
        }
      }
      const suggest = { item: this.suggest_num, eat: eat, bite: bite };
      this.suggest_num = "";
      this.suggests_log.push(suggest);
      if (eat === 4) {
        this.clear = true;
      }
    }
  }
};
</script>

<style scoped>
.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  display: block;
  font-weight: 400;
  font-size: 100px;
  color: #2E495E;
  letter-spacing: 1px;
  font-size: 6em;
}
.green {
  color: #00C48D;
}

.subtitle {
  font-weight: 300;
  font-size: 3em;
  color: #2E495E;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
