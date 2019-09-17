<template>
  <div>
    <h1>{{result}}</h1>
    <form @submit.prevent="onSubmitForm">
      <input ref="answer" minlength="4" maxlength="4" v-model="value" />
      <button>입력</button>
    </form>
    <div>시도: {{tries.length}}</div>
    <ul>
      <li v-for="t in tries">
        <div>{{t.try}}</div>
        <div>{{t.result}}</div>
      </li>
    </ul>
  </div>
</template>

<script>
const getNumbers = () => {
  const array = [];
  while (true) {
    const chosen = Math.floor(Math.random() * 9, 1);
    if (array.indexOf(chosen) > 0) {
      continue;
    }
    array.push(chosen);

    if (array.length == 4) {
      break;
    }
  }
  console.log(array);
  return array;
};

export default {
  data() {
    return {
      answer: getNumbers(), // ex) [1,4,3,5]
      tries: [],
      value: "",
      result: ""
    };
  },
  methods: {
    onSubmitForm() {
      if (this.value === this.answer.join("")) {
        this.tries.push({
          try: this.value,
          result: "홈런"
        });
        this.result = "홈런";
        alert("게임을 다시 시작합니다.");
        this.value = "";
        this.answer = getNumbers();
        this.tries = [];
        this.$refs.answer.focus();
      } else {
        if (this.tries.length >= 9) {
          //10번째 시도
          this.result = `10번 넘게 틀려서 실패! 답은 ${this.answer.join(
            ","
          )} 였습니다.`;
          alert("게임을 다시 시작합니다.");
          this.answer = getNumbers();
          this.tries = [];
          this.$refs.answer.focus();
        }
        let strike = 0;
        let ball = 0;
        const answerArray = this.value.split("").map(v => parseInt(v));
        for (let i = 0; i < 4; i += 1) {
          if (answerArray[i] === this.answer[i]) {
            // 숫자 자리수 모두 정답
            strike++;
          } else if (this.answer.includes(answerArray[i])) {
            // 숫자만 정답
            ball++;
          }
        }
        this.tries.push({
          try: this.value,
          result: `${strike} 스트라이크, ${ball} 볼입니다.`
        });
        this.value = "";
        this.$refs.answer.focus();
      }
    }
  }
};
</script>

<style scoped>
</style>