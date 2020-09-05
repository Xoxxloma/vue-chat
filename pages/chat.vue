<template>
  <div class="c-wrap">
    <div class="c-chat" ref="block">
      <Message
        v-for="mes in messages"
        :key="mes.text"
        :name="mes.name"
        :text="mes.text"
        :owner="mes.id === user.id"
      />
    </div>
    <div class="c-form">
      <ChatForm />
    </div>
  </div>
</template>

<script>
import { mapState } from "vuex";
import Message from "../components/message";
import ChatForm from "../components/chat-form";
export default {
  middleware: ["chat"],
  computed: mapState(["user", "messages"]),
  watch: {
    messages() {
      setTimeout(() => {
        this.$refs.block.scrollTop = this.$refs.block.scrollHeight;
      });
    }
  },
  head() {
    return {
      title: `room ${this.user.room}`
    };
  },
  components: { Message, ChatForm }
};
</script>

<style scoped>
.c-wrap {
  height: 100%;
  position: relative;
  overflow: hidden;
}

.c-form {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 1rem;
  height: 80px;
  background: #212121;
}

.c-chat {
  position: absolute;
  top: 0;
  right: 0;
  left: 0;
  bottom: 80px;
  padding: 1rem;
  overflow-y: auto;
}
</style>
