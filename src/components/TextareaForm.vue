<script lang="tsx">
  import { defineComponent, ref, onMounted } from 'vue'

  function isMobile() {
    return document.body.offsetWidth < 769;
  }

  export default defineComponent({
    emits: ['onSubmit'],
    setup(props, { emit }) {
      const textareaRef = ref<HTMLTextAreaElement>()
      const insertRef = ref<HTMLDivElement>()
      const submitDisabled = ref(true)
      const textareaInitHeight = ref('0px');

      // 更新 textarea 的高度
      const updateTextareaHeight = () => {
        const textareaEl = textareaRef.value
        const insertEl = insertRef.value

        if (!textareaEl) {
          return
        }
        
        // https://stackoverflow.com/a/24676492/2777142
        textareaEl.style.height = '5px'
        let newHeight = Math.min(textareaEl.scrollHeight, 260)
        const initHeight = Number(textareaInitHeight.value.replace('px', ''))

        if (newHeight < initHeight) {
          newHeight = initHeight
        }
        
        textareaEl.style.height = `${newHeight}px`
        if (insertEl) {
          insertEl.style.height = `${newHeight + 20}px`
        }
      }

      // 更新 submit 按钮的 disable 态
      const updateSubmitDisabled = () => {
        const textareaEl = textareaRef.value
        const value = textareaEl?.value?.trim()
        submitDisabled.value = !value
      }

      // 输入内容触发
      const handleInput = (e: Event) => {
        updateTextareaHeight()
        updateSubmitDisabled()
      }

      // 修改回车默认行为
      const handleKeydown = (e: KeyboardEvent) => {
        // [Ctrl/Cmd + Enter] 发送消息
        if (e.key === 'Enter' && (e.ctrlKey || e.metaKey)) {
          e.preventDefault()
          handleSubmit()
          return
        }

        // PC 端，回车发送消息
        if (!isMobile() && e.key === 'Enter' && !e.shiftKey && !e.ctrlKey && !e.altKey) {
          e.preventDefault()
          handleSubmit()
          return
        }
      }

      // 提交表单处理
      const handleSubmit = (e?: Event) => {
        e?.preventDefault()
        const value = textareaRef.value?.value.trim()
        if (!value) {
          return
        }

        // 提交后清空内容
        if (textareaRef.value?.value) {
          textareaRef.value.value = ''
        }

        updateTextareaHeight()
        updateSubmitDisabled()
        emit('onSubmit', value)
      }

      onMounted(() => {
        textareaInitHeight.value = getComputedStyle(textareaRef.value as HTMLTextAreaElement).height
      })

      return () => (
        <>
          <div class="insert">
            <div ref={insertRef} class="block"></div>
          </div>
          <div class="form-wrapper">
            <form onSubmit={handleSubmit}>
              <textarea
                ref={textareaRef}
                rows={1}
                placeholder=""
                onInput={handleInput}
                onKeydown={handleKeydown}
              />
              <input type="submit" value="发送" disabled={submitDisabled.value} />
            </form>
          </div>
        </>
      )
    }
  })
</script>

<style lang="scss" scoped>
  .form-wrapper {
    position: fixed;
    bottom: 0;
    width: 100%;
    padding: 0.625rem 0.75rem calc(0.625rem + env(safe-area-inset-bottom)) 0.75rem;
    background-color: rgb(243 244 246 / 1);
    box-sizing: border-box;

    form {
      display: flex;
      margin: 0;

      textarea {
        flex-grow: 1;
        padding: 0.5rem 0.75rem;
        border-radius: 0.25rem;
        border: 1px solid transparent;
        outline: none;
        font-size: 100%;
        font-family: inherit;
        line-height: inherit;
        font-weight: inherit;
        resize: none;
        overflow: hidden;

        &:focus-visible {
          border: 1px solid #f59e0b;
        }
      }

      input {
        width: 3.5rem;
        padding: 0.5rem 0.75rem;
        margin-left: 0.75rem;
        border-radius: 0.25rem;
        font-size: 100%;
        background-color: #fff;
        background-image: none;
        outline: none;
        border: none;
        cursor: pointer;

        &:disabled {
          color: rgb(156 163 175 / 1);
          background-color: rgb(229 231 235 / 1);
          cursor: not-allowed;
        }
      }
    }
  }

  .insert {
    padding-bottom: env(safe-area-inset-bottom);

    .block {
      height: 3.75rem;
      margin-bottom: env(safe-area-inset-bottom);
    }
  }
</style>