<template>
<div class="inbox-body">
    <div class="mail-option">
        <div class="btn-group">
            <a href="#" class="btn" @click="refresh">
                <i class="fa fa-refresh"></i>&nbsp; Refresh
            </a>
        </div>
    </div>

    <app-messages :messages="incomingMessages"></app-messages>
</div>
</template>

<script>
import { eventBus } from './main'

import Messages from './Messages.vue'

export default {
    components: {
        appMessages: Messages
    },
    props: {
        data: {
            type: Object,
            required: true
        }
    },
    computed: {
        incomingMessages() {
            return this.data.messages.filter(message => message.type === 'incoming' && !message.isDeleted)
        }
    },
    methods: {
        refresh() {
            eventBus.$emit('refreshMessages')
        }
    }
}
</script>