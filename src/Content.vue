<template>
<aside class="lg-side">
  <div class="inbox-head">
    <h3>{{ currentView.title }}</h3>
  </div>
  <keep-alive>
    <component :is="currentView.tag" :data="currentView.data"></component>
  </keep-alive>
</aside>
</template>

<script>
import { eventBus } from './main'

import Inbox from './Inbox.vue'
import Sent from './Sent.vue'
import Important from './Important.vue'
import Trash from './Trash.vue'
import ViewMessage from './ViewMessage.vue'

export default {
  components: {
    appInbox: Inbox,
    appSent: Sent,
    appImportant: Important,
    appTrash: Trash,
    appViewMessage: ViewMessage
  },
  created() {
    eventBus.$on('changeView', data => {
      this.history = [{
        tag: data.tag || '',
        title: data.title || '',
        data: data.data || {}
      }, ...this.history]
      // let temp = [{
      //   tag: data.tag,
      //   title: data.title,
      //   data: data.data || {}
      // }]
      // this.history = temp.concat(this.history.splice(0))
    })
  },
  props: {
    messages: {
      type:Array,
      required: true
    }
  },
  data() {
    return {
      history: [
        {
          tag: 'app-inbox',
          title: 'inbox',
          data: {
            messages: null
          }
        }
      ]
    }
  },
  computed: {
    currentView() {
      const current = this.history[0]
      current.data.messages = this.messages
      return current
    },
    previousView() {
      return this.history[1] ? this.history[1] : null;
    }
  }
}
</script>