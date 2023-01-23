<template>
    <div class="modal" v-if="show" @click="hideModal"> 
        <div @click.stop class="modal__content">
            <slot></slot>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
    export default defineComponent({
        name: 'modal',
        props: {
            show: {
                type: Boolean,
                default: false
            }
        },
        methods: {
            hideModal() {
                this.$emit('update:show', false)
            }
        }
    })
</script>

<style scoped>
    .modal {
        top: 0;
        right: 0;
        bottom: 0;
        left: 0;
        position: fixed;
        display: flex;
        justify-content: center;
        background-color: rgb(0, 0, 0, 0.5);
    }

    .modal__content {
        display: flex;
        align-items: center;
        width: 500px;
        height: 200px;
        margin: auto;
        border-radius: 10px;
        background-color: rgb(255, 255, 255);
        padding: 10px;
    }
</style>