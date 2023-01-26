<template>
    <div class="game-inner-wrapper">
        <div class="title">
            {{ post.title }}
        </div>
        <div class="value">
            <value-item v-for="clue in post.clues" :post="clue" @custom-change="handleChange" />
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
    },
    setup(props, {emit}) {
        const handleChange = (score: number) => {
            emit('updateScore', score)
        }

        return {
            handleChange
        }
    }
})
</script>

<style scoped>
    .game-inner-wrapper {
        display: flex;
        align-items: center;
    }

    .title {
        width: 250px;
        height: 70px;
        padding: 10px;
        color: #fff;
        font-size: 20px;
        font-weight: bold;
        border-radius: 5px;
        display: flex;
        align-items: center;
        text-transform: capitalize;
    }

    .value {
        display: flex;
        align-items: center;
    }
</style>