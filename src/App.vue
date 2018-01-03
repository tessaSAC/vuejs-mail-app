<template>
<div class="container">
  <div class="mail-box">
    <app-sidebar :messages="messages"></app-sidebar>
    <app-content :messages="messages"></app-content>
  </div>
</div>
</template>

<script>
import Sidebar from './Sidebar.vue'
import Content from './Content.vue'

import messages from './data/messages'
import randomMessages from './data/random-messages'
import { eventBus } from './main'

export default {
  created() {
    eventBus.$on('sentMessage', data => {
      this.messages = [data.message, ...this.messages]
    })

    eventBus.$on('refreshMessages', () => {
      const randomIdx = Math.floor(Math.random() * randomMessages.length)
      this.messages = [randomMessages[randomIdx], ...this.messages]
    })
  },
  data() {
    return {
      messages
    }
  },
  components: {
    appContent: Content,
    appSidebar: Sidebar
  }

            eventBus.$on('sentMessage', (data) => {
                let temp = [data.message];
                this.messages = temp.concat(this.messages.slice(0));
            });
        },
        components: {
            appSidebar: Sidebar,
            appContent: Content
        }
    }
</script>