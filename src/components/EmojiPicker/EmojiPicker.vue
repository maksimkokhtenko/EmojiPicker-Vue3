<template>
    <div class="emojipicker">
        <label class="label-input">Demo</label>
        <input ref="input" type="text"/>
        <div class="emojipicker__inner">
            <span class="emojipicker__icon" @click.prevent="togglePicker">😊</span>
            <div class="emojipicker__container" :class="{ open: isOpen }">
                <input v-if="enableSearch" ref="search" type="text" v-model="searchQuery" placeholder="Search emojis..."/>
                <div v-for="category in displayCategories" :key="category" class="emojipicker__category">
                    <span v-if="category !== 'Frequently used' || hasFrequentlyUsedEmojis">{{ category }}</span>
                    <div class="emojipicker__emojis">
                        <button
                            v-for="(emoji, index) in filteredEmojis[category]"
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
import { ref, computed, onMounted } from 'vue'
import emojis from './emojis.json'
import { defineProps } from 'vue'

const props = defineProps({
    enableSearch: {
        type: Boolean,
        default: true,
    },
    enableFrequentlyUsed: {
        type: Boolean,
        default: true,
    }
})

const input = ref(null)
const search = ref(null)
const isOpen = ref(false)
const searchQuery = ref('')
const frequentlyUsed = ref(JSON.parse(localStorage.getItem('frequentlyUsed')) || {})

const categories = computed(() => Object.keys(emojis))

const hasFrequentlyUsedEmojis = computed(() => props.enableFrequentlyUsed && frequentlyUsed.value['Frequently used'] && frequentlyUsed.value['Frequently used'].length > 0)

const filteredEmojis = computed(() => {
    const query = searchQuery.value.toLowerCase()
    const result = {}

    if (hasFrequentlyUsedEmojis.value) {
        result['Frequently used'] = frequentlyUsed.value['Frequently used']
    }

    categories.value.forEach(category => {
        const filtered = Object.entries(emojis[category]).filter(([name, emoji]) =>
            name.toLowerCase().includes(query) || emoji.includes(query)
        )

        if (filtered.length > 0) {
            result[category] = Object.fromEntries(filtered)
        }
    })

    return result
})

const displayCategories = computed(() => {
    const categoriesToShow = hasFrequentlyUsedEmojis.value ? ['Frequently used', ...categories.value] : categories.value
    return categoriesToShow.filter(category => filteredEmojis.value[category] && Object.keys(filteredEmojis.value[category]).length > 0)
})

const handleEmojiClick = emoji => {
    input.value.value += emoji
    input.value.focus()
    isOpen.value = false
    if (props.enableFrequentlyUsed) {
        updateFrequentlyUsed(emoji)
    }
}

const updateFrequentlyUsed = emoji => {
    const frequentlyUsedEmojis = frequentlyUsed.value['Frequently used'] || []
    const emojiIndex = frequentlyUsedEmojis.indexOf(emoji)

    if (emojiIndex !== -1) {
        frequentlyUsedEmojis.splice(emojiIndex, 1)
    }

    frequentlyUsedEmojis.unshift(emoji)

    if (frequentlyUsedEmojis.length > 10) {
        frequentlyUsedEmojis.pop()
    }

    frequentlyUsed.value['Frequently used'] = frequentlyUsedEmojis
    localStorage.setItem('frequentlyUsed', JSON.stringify(frequentlyUsed.value))
}

const togglePicker = () => {
    isOpen.value = !isOpen.value
}

onMounted(() => {
    frequentlyUsed.value = JSON.parse(localStorage.getItem('frequentlyUsed')) || {}
})
</script>