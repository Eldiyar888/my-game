<template>
    <div class="game-inner-wrapper">
        <div class="title">
            {{ post.title }}
        </div>
        <div class="value">
            <value-item v-for="clue in post.clues" :post="clue" />
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import ValueItem from './ValueItem.vue';
export default defineComponent({
    props: {
        post: {
            type: <any[]>[],
            required: true
        }
    },
    components: {
        ValueItem
    }
})
</script>

<style scoped>
    .game-inner-wrapper {
        display: flex;
        align-items: center;
    }

    .title {
        width: 200px;
        padding: 10px;
        color: #fff;
        font-size: 25px;
        font-weight: bold;
    }

    .value {
        display: flex;
        align-items: center;
    }
</style>