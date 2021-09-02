<template>
  <div class="nickname-generator">
    <h1>あだ名決定ルーレット</h1>
    <form @submit.prevent="submit">
      <label for="family-name">
        <span>名字(ひらがな)</span>
        <input id="family-name" v-model="familyName" type="text" />
      </label>
      <label for="first-name">
        <span>名前(ひらがな)</span>
        <input id="first-name" v-model="firstName" type="text" />
      </label>
      <button type="submit">あだ名は？</button>
    </form>
    <div v-if="showResult" class="result">
      今日から君は
      <br />
      <span class="nickname">{{ nickname }}</span>
      <br />
      だ！
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

const suffixVariations = [
  ['っちゃん', 'ーちゃん', 'ーくん'],
  ['ちゃん', 'くん', 'やん', 'べえ', 'ピー', 'セン', 'ぽん', 'キング'],
  ['ちゃん', 'くん', 'ーぬ', 'マン'],
]

export default Vue.extend({
  data() {
    const familyName = ''
    const firstName = ''
    const cutCountFromName = 1
    const isTargetNameFamilyName = true
    const showResult = false
    return {
      familyName,
      firstName,
      cutCountFromName,
      isTargetNameFamilyName,
      showResult,
    }
  },
  computed: {
    nickname(): string {
      const baseName = this.isTargetNameFamilyName
        ? this.familyName
        : this.firstName
      if (baseName.length < 1) {
        return '名無し'
      }

      const cutName = baseName.slice(0, this.cutCountFromName)
      const variations = suffixVariations[cutName.length - 1]
      const suffix = variations[Math.floor(Math.random() * variations.length)]
      return `${cutName}${suffix}`
    },
  },
  methods: {
    changeTargetName() {
      const min = 0
      const max = 1
      const coin = Math.floor(Math.random() * (max + 1 - min)) + min
      this.isTargetNameFamilyName = coin === 0
    },
    changeCutCountFromName() {
      const min = 1
      const max = 3
      this.cutCountFromName = Math.floor(Math.random() * (max + 1 - min)) + min
    },
    submit() {
      this.changeTargetName()
      this.changeCutCountFromName()
      this.showResult = true
    },
  },
})
</script>

<style scoped>
.nickname-generator {
  display: flex;
  flex-direction: column;
  align-items: center;
  max-width: 400px;
  margin: 0 auto;
}

label {
  display: block;
}

label + label {
  margin-top: 20px;
  margin-bottom: 40px;
}

button {
  display: block;
  margin-right: auto;
  margin-bottom: 30px;
  margin-left: auto;
}

.result {
  font-size: 20px;
  text-align: center;
}

.nickname {
  font-size: 36px;
}
</style>
