<script lang="tsx">
  import { defineComponent, PropType } from 'vue'

  import { MessageProps } from '../types'
  import { formatMessage, FormatMessageMode } from '../utils/formatMessage'

  import avatarAI from '../assets/avatar-ai.png'
  import avatarUser from '../assets/avatar-user.svg'

  export default defineComponent({
    props: {
      avatar: {
        type: String as PropType<MessageProps['avatar']>,
      },
      content: {
        type: String as PropType<MessageProps['content']>,
      },
      align: {
        type: String as PropType<MessageProps['align']>,
        default: 'left',
      },
    },

    setup(props) {
      return () => (
        <div class={`message align-${props.align}`}>
          {props.align === 'left' ?
            <img class="avatar" src={avatarAI} alt="avatar" />
            :
            <img class="avatar" src={avatarUser} alt="avatar" />
          }
          <div class="content">
            <p
              v-html={
                formatMessage(
                  props.content,
                  props.avatar === 'ChatAI' ? FormatMessageMode.partial : FormatMessageMode.zero,
                )
              }
            />
          </div>
        </div>
      )
    }
  })
</script>

<style lang="scss" scoped>
  .message {
    display: flex;
    margin: 1rem 0;
    padding: 0 0.75rem;

    .avatar {
      width: 2.5rem;
      height: 2.5rem;
      border-radius: 0.25rem;
      background-color: #fff;
    }

    .content {
      margin: 0 0.75rem;
      padding: 0.5rem 0.75rem;
      border-radius: 0.25rem;
      background-color: #fff;
      font-size: 100%;
    }

    &.align-right {
      flex-direction: row-reverse;

      .content {
        margin: 0 0.75rem;
        padding: 0.5rem 0.75rem;
        background-color: #abe987;
      }
    }
  }
</style>
