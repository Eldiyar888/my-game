<template>
    <div class="game-inner-wrapper">
        <div class="title">
            {{ post.title }}
        </div>
        <div class="value">
            <value-item v-for="clue in post.clues" :post="clue" :totalValue="totalValue" />
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
        },
        totalValue: {
            type: Object
        }
    },  
    components: {
        ValueItem
    },
    setup() {
    }
})
</script>

<style scoped>
    .game-inner-wrapper {
        height: 100%;
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