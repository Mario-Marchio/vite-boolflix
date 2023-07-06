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
    <div class="bg-black">
        <div class="d-flex container justify-content-between ">
            <h1 class="text-danger">BOOLFLIX</h1>
            <form class="d-flex align-items-center" @submit.prevent="$emit('form-submit')">
                <input v-model.trim="searchTerm" @keyup="$emit('term-change', searchTerm)" type="text"
                    placeholder="scrivi qui...">
                <button>Cerca</button>
            </form>
        </div>
    </div>
</template>