<template>
  <Header />
  <div>
    <Message
      v-for="message in messages"
      :key="message.id"
      :avatar="message.avatar"
      :align="message.align"
      :content="message.content"
    />
    <Message
      v-if="chatLoading"
      avatar="ChartAI"
      align="left"
      :content="LOADING_MESSAGE"
    />
  </div>
  <Textareaform @onSubmit="handleSubmit" />
</template>

<script setup lang="ts">
  import { ref } from 'vue'

  import Header from './components/Header.vue'
  import Message from './components/Message.vue'
  import Textareaform from './components/TextareaForm.vue'

  import { MessageProps } from './types'
  import { scrollToBottom } from './utils/scroll'

  const WELCOME_MESSAGE = '你好！有什么我可以帮助你的吗？'
  const LOADING_MESSAGE = '正在努力思考...'

  const chatLoading = ref(false)
  const messages = ref<MessageProps[]>([
    {
      id: String(Math.random()),
      avatar: 'ChatAI',
      align: 'left',
      content: WELCOME_MESSAGE,
    },
  ])

  const sleep = (time: number) => new Promise((resolve) => setTimeout(resolve, time))

  const handleSubmit = async (value: string) => {
    messages.value.push({
      id: String(Math.random()),
      avatar: 'user',
      align: 'right',
      content: value,
    })
    chatLoading.value = true
    await sleep(16)
    scrollToBottom()

    await sleep(1000)
    messages.value.push({
      id: String(Math.random()),
      avatar: 'ChatAI',
      align: 'left',
      content: WELCOME_MESSAGE,
    })

    chatLoading.value = false

    await sleep(16)
    scrollToBottom()
  }
</script>

<style scoped>
</style>
