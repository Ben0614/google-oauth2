<script setup lang="ts">
// onMounted
import { ref } from 'vue'
// 一鍵登入
// import { googleOneTap } from 'vue3-google-login'
// 獲取access_token
import { googleTokenLogin } from 'vue3-google-login'
// 獲取auth_code
import { googleAuthCodeLogin } from 'vue3-google-login'
import { decodeCredential } from 'vue3-google-login'

// 1.install vue3-google-login
// 2.到main.ts裡設定

/*
  Google Cloud https://cloud.google.com/?hl=zh-tw 從裡面創建OAuth相關的東西
  前往控制台 > API和服務 > 憑證 > 如果已經有OAuth2.0 用戶端ID憑證 就直接用 沒有就建立憑證 
  > 用戶端編號填入下面的GOOGLE_CLIENT_ID  還有main.ts
*/


const GOOGLE_CLIENT_ID = ''

const data = ref()

// 基本的回調 可設置獲取credential(憑證) 或 access_token(訪問令牌)
const callback = <T>(result: T) => {
  data.value = result
  console.log('callback_result', result);
}

// 獲取 access_token (訪問令牌)
const handleGoogleAccessTokenLogin = async () => {
  try {
    const result = await googleTokenLogin({
      clientId: GOOGLE_CLIENT_ID
    })

    data.value = result

    console.log('googleTokenLogin_result', result);
  } catch (err) {
    console.log(err);
  }
}

// 獲取 auth_code
const handleGoogleAuthCodeLogin = async () => {
  try {
    const result = await googleAuthCodeLogin({
      clientId: GOOGLE_CLIENT_ID
    })

    data.value = result

    console.log('googleAuthCodeLogin_result', result);
  } catch (err) {
    console.log(err);
  }
}

// 登入並拿到用戶資料
const loginAndProfile = (response:any) => {
  // decodeCredential will retrive the JWT payload from the credential
  const userData = decodeCredential(response.credential)
  console.log("Handle the userData", userData)
}

// 一鍵登入 調用 (不須按鈕)
// onMounted(async () => {
//   try {
//     const result = await googleOneTap()
//     console.log('googleOneTap_result', result);
//   } catch (err) {
//     console.log(err);
//   }
// })
</script>

<template>
  <h1>Google OAuth2.0</h1>

  <!-- 一鍵登入 加上prompt就會自動調用 (出現在右上角) -->
  <div class="mb">
    <GoogleLogin :callback="callback" prompt />
  </div>
  <!-- 回傳credential (憑證) -->
  <div class="mb">
    <GoogleLogin :callback="callback" />
  </div>
  <!-- 用戶資料  -->
    <div class="mb">
      <GoogleLogin :callback="loginAndProfile" prompt auto-login/>
    </div>
  <!-- 回傳access_token (訪問令牌) -->
  <div class="mb">
    <GoogleLogin :callback="callback" popup-type="TOKEN">
      <button class="border">使用 Google 進行登入 (callback TOKEN)</button>
    </GoogleLogin>
  </div>
  <!-- 回傳access_token (訪問令牌) -->
  <div class="mb">
    <button type="button" @click="handleGoogleAccessTokenLogin" class="border">
      使用 Google 進行登入 (googleTokenLogin)
    </button>
  </div>
  <!-- 回傳 auth_code  -->
  <div class="mb">
    <button type="button" @click="handleGoogleAuthCodeLogin" class="border">
      使用 Google 進行登入 (googleAuthCodeLogin)
    </button>
  </div>
</template>

<style scoped>
.mb {
  margin-bottom: 12px
}

.border {
  border: 1px solid black
}

p {
  margin-top: 12px;
  word-break: break-all;
}

.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
