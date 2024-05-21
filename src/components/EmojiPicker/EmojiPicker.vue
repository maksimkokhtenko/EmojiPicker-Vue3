<template>
    <div class="emojipicker">
        <label class="label-input">Demo</label>
        <input ref="input" type="text"/>
        <div class="emojipicker__inner">
            <span class="emojipicker__icon" @click.prevent="togglePicker">😊</span>
            <div class="emojipicker__container" :class="{ open: isOpen }">
                <div v-for="category in categories" :key="category" class="emojipicker__category">
                    <span>{{ category }}</span>
                    <div class="emojipicker__emojis">
                        <button
                            v-for="(emoji, index) in emojis[category]"
                            :key="index"
                            @click.prevent="handleEmojiClick(emoji)"
                        >
                            {{ emoji }}
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import emojis from './emojis.json'

const input = ref(null)
const isOpen = ref(false)

const categories = computed(() => Object.keys(emojis))

const handleEmojiClick = emoji => {
    input.value.value += emoji
    input.value.focus()
    isOpen.value = false
}

const togglePicker = () => {
    isOpen.value = !isOpen.value
}
</script>