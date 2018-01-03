<template>
<div>
  <table v-if="messages.length" class="table table-inbox table-hover">
    <tbody>
      <tr v-for="(message, idx) in messages" :key="idx" @click="openMessage(message)" :class="{ unread: message.isRead === false }">
        <td input type="checkbox"></td>
        <td>
          <a href="#" v-if="typeof message.isImportant !== 'undefined'" @click.prevent.stop="message.isImportant = !message.isImportant">
            <i :class="['fa', 'fa-star', { important: message.isImportant }]"></i>
          </a>
        </td>
        <td>{{ message.from.name }}</td>
        <td>{{ message.subject }}</td>
        <td><i v-if="message.attachments.length" class="fa fa-paperclip"></i></td>
        <td class="text-right">{{ message.date.fromNow() }}</td>
      </tr>
    </tbody>
  </table>

  <p v-else>No messages here yet.</p>
</div>
</template>

<script>
import { eventBus } from './main'

export default {
  props: {
    messages: {
      type: Array,
      required: true
    }
  },
  methods: {
    openMessage(message) {
      eventBus.$emit('changeView', {
        tag: 'app-view-message',
        title: message.subject,
        data: {
          message
        }
      })
    }
  }
}
</script>
