<script lang="tsx">
  import { defineComponent } from 'vue'

  export default defineComponent({
    setup() {
      return () => (
        <>
          <header class="header">
            <div class="title">Message</div>
          </header>
          <div class="insert"></div>
        </>
      )
    }
  })
</script>

<style lang="scss" scoped>
  .header {
    position: fixed;
    top: 0;
    width: 100%;
    height: 3rem;
    display: flex;
    justify-content: center;
    align-items: center;
    border-bottom: 1px solid rgb(209 213 219 / 1);
    background-color: rgb(237 237 237 / 1);

    .title {
      font-size: 1rem;
      font-weight: bold;
    }
  }

  .insert {
    height: 3rem;
  }
</style>
