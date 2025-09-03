<script setup lang="ts">
import { ref } from 'vue'

const message = ref('Click the button to clear cookies.')
const isLoading = ref(false)

async function clearCookies() {
  isLoading.value = true
  message.value = 'Working...'
  console.log('--- Starting to clear cookies (Base Domain Method) ---')

  try {
    const [tab] = await chrome.tabs.query({ active: true, currentWindow: true })
    if (!tab?.url) {
      throw new Error('Cannot access cookies for this page.')
    }

    // --- ФІНАЛЬНА ЛОГІКА ---
    const url = new URL(tab.url)
    const hostnameParts = url.hostname.split('.')
    // Беремо останні два елементи (напр. 'youtube' та 'com') і з'єднуємо їх
    const baseDomain = hostnameParts.slice(-2).join('.')

    console.log(`Querying cookies for base domain: .${baseDomain}`)

    // Запитуємо кукі, використовуючи БАЗОВИЙ домен
    const cookies = await chrome.cookies.getAll({ domain: baseDomain })
    console.log('Cookies found for this domain:', cookies)
    // --------------------

    if (cookies.length === 0) {
      message.value = 'No cookies found on this site.'
      isLoading.value = false
      return
    }

    let cookiesRemoved = 0
    for (const cookie of cookies) {
      const cookieUrl = `http${cookie.secure ? 's' : ''}://${cookie.domain}${cookie.path}`
      await chrome.cookies.remove({
        url: cookieUrl,
        name: cookie.name,
        storeId: cookie.storeId,
      })
      cookiesRemoved++
    }

    message.value = `Successfully removed ${cookiesRemoved} cookie(s)!`
  } catch (error) {
    message.value = `Error: ${error.message}`
  } finally {
    isLoading.value = false
  }
}

// --- ДОДАЙТЕ ЦЮ НОВУ ФУНКЦІЮ ---
async function testCookieAccess() {
  console.log('--- Running Cookie Access Test ---')
  try {
    const [tab] = await chrome.tabs.query({ active: true, currentWindow: true })
    if (!tab?.url) {
      console.error('Test failed: Could not get tab URL.')
      return
    }

    const testCookie = {
      url: tab.url,
      name: 'test_by_extension',
      value: '12345',
    }

    console.log('Attempting to set cookie:', testCookie)
    await chrome.cookies.set(testCookie)
    console.log('Cookie should be set. Now trying to get it back...')

    const retrievedCookie = await chrome.cookies.get({
      url: tab.url,
      name: 'test_by_extension',
    })

    console.log('Retrieved cookie:', retrievedCookie)
    if (retrievedCookie) {
      console.log('✅ SUCCESS: The extension can set and get cookies!')
      message.value = 'Test successful!'
      // Clean up the test cookie
      await chrome.cookies.remove({ url: tab.url, name: 'test_by_extension' })
    } else {
      console.error('❌ FAILURE: The extension could NOT get the cookie it just set.')
      message.value = 'Test failed. Check console.'
    }
  } catch (error) {
    console.error('An error occurred during the test:', error)
    message.value = 'Test failed with an error.'
  }
}
</script>

<template>
  <main>
    <h3>Cookie Cleaner</h3>
    <button @click="clearCookies" :disabled="isLoading">
      {{ isLoading ? 'Deleting...' : 'Clear Cookies for this site' }}
    </button>

    <button @click="testCookieAccess" style="background-color: #f0ad4e; margin-top: 10px">
      Run Cookie Access Test
    </button>

    <p class="message">{{ message }}</p>
  </main>
</template>

<style scoped>
main {
  width: 250px;
  padding: 1rem;
  text-align: center;
  font-family: sans-serif;
  color: #2c3e50;
}
button {
  width: 100%;
  padding: 0.8rem;
  border: none;
  background-color: #42b883;
  color: white;
  font-size: 1rem;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.2s;
}
button:hover {
  background-color: #34966a;
}
button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}
.message {
  margin-top: 1rem;
  min-height: 20px;
  font-size: 0.9rem;
}
</style>
