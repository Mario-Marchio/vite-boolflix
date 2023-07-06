<script>
export default {
    data() {
        return {
            searchTerm: ''
        }
    },
    emits: ['form-submit', 'term-change']
}
</script>
<template>
    <form @submit.prevent="$emit('form-submit')">
        <input v-model.trim="searchTerm" @keyup="$emit('term-change', searchTerm)" type="text" placeholder="scrivi qui...">
        <button>Cerca</button>
    </form>
</template>