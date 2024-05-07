<template>
  <div>
    <div>
      {{inputText}}
      <input type="text" v-model="inputText" /><button @click="submit">
        发送
      </button>
    </div>
    <div>{{ result}}</div>
  </div>
</template>

<script setup>
import axios from "axios";
import { ref } from "vue";
const apiKey = "AIzaSyAr3w_0G9QvdxF7I3N3b9t30oChxJEXXrg"; // 替换为你的 API key
const inputText = ref("");
const result = ref("");
const submit=async ()=>{
  const data = {
  contents: [
    {
      parts: [
        {
          text: inputText.value,
        },
      ],
    },
  ],
};
const config = {
  method: "post",
  url: `https://generativelanguage.googleapis.com/v1beta/models/gemini-pro:generateContent?key=${apiKey}`,
  headers: {
    "Content-Type": "application/json",
  },
  data: data,
};

let res= await axios(config)
  .then((response) => {
    console.log(response.data);
    let {data:{candidates:{0:{content:{parts:{0:{text}}}}}}}=response;   
    return text;
  })
  .catch((error) => {
    console.error(error);
  });
  console.log(res);
  result.value =res;
  
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
</style>
