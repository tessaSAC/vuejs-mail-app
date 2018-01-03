<template>
<div class="inbox-body">
  <div class="mail-option">
    <button class="btn btn-primary" @click="navigateBack()">
      <i class="fa fa-arrow-left" aria-hidden="true"></i>&nbsp; Back
    </button>

    <button class="btn btn-danger" @click="data.message.isDeleted = true" :disabled="data.message.isDeleted">
      <i class="fa fa-trash-o"></i>&nbsp; {{ data.message.isDeleted ? 'Deleted' : 'Delete' }}
    </button>

    <template v-if="data.message.hasOwnProperty('isRead')">
      <button class="btn btn-primary" @click="data.message.isRead = false" :disabled="!data.message.isRead">
        <i class="fa fa-envelope-open" aria-hidden="true"></i>&nbsp; Mark as unread
      </button>

      <button class="btn btn-primary" @click="data.message.isRead = true" :disabled="data.message.isRead">
        <i class="fa fa-envelope" aria-hidden="true"></i>&nbsp; Mark as read
      </button>
    </template>
  </div>

  <p><strong>Date</strong> {{ data.message.date.fromNow() }}</p>
  <p><strong>From:</strong> {{ data.message.from.name }} <{{ data.message.from.email }}></p>
  <hr>
  <div v-html="data.message.content" class="message"></div>

  <div v-if="data.message.attachments.length" class="attachments">
    <h4>Attachments</h4>

    <ul>
      <li v-for="(attachment, idx) in data.message.attachments" :key="idx">
        <i class="fa fa-paperclip"></i> {{ attachment.fileName }} ({{ attachment.size | formatBytes }})
      </li>
    </ul>
  </div>
</div>
</template>

<script>
import { eventBus } from './main'

export default {
  activated() {
    if (this.data.message.hasOwnProperty('isRead')) this.data.message.isRead = true
  },
  props: {
      data: {
          type: Object,
          required: true
      }
  },
  methods: {
    navigateBack() {
      const previousView = this.$parent.previousView  // Not great bc couples this component to its parent tightly
      eventBus.$emit('changeView', {
        tag: previousView.tag,
        title: previousView.title,
        data: previousView.data
      })
    }
  },
  filters: {
      formatBytes(bytes) {
          if (!bytes) return '0 bytes'

          const decimals = 2
          const k = 1000
          const dm = decimals + 1 || 3  // Why `|| 3`?
          const sizes = ['Bytes', 'KB', 'MB', 'GB', 'TB', 'PB', 'EB', 'ZB', 'YB']
          const i = Math.floor(Math.log(bytes) / Math.log(k))

          return parseFloat((bytes / Math.pow(k, i)).toFixed(dm)) + ' ' + sizes[i]
      }
  }
}
</script>