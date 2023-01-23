<template>
    <div>
        <div class="value-inner" @click="showModal">
            {{ post.value == null ? 'пусто': post.value }}
        </div>
        <modal v-model:show="modalVisible">
            <div class="main-block">
                <div class="block-question">
                    {{ post.question }}
                </div>
                <div class="block-input">
                    <div class="block-input-inner-wrapper">
                        <input class="input-modal" v-model="answer" type="text" placeholder="Введите ответ" />
                        <button class="btn-check" @click="checkAnswer">Подтвердить</button>
                    </div>
                    <div>{{ msg }}</div>
                </div>
            </div>
        </modal>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Modal from './Modal.vue';
export default defineComponent({
    components: {
        Modal
    },
    data() {
        return {
            modalVisible: false,
            answer: '',
            msg: ''
        }
    },
    props: {
        post: {
            type: Object,
            required: true
        }
    },
    methods: {
        showModal() {
            this.modalVisible = true;
        },
        checkAnswer() {
            if (this.answer == this.post.answer) {
                this.msg = 'right';
            } else {
                this.msg = 'wrong';
            }
        }
    }
})
</script>

<style scoped>

.main-block {
    width: 100%;
}

.value-inner {
    display: flex;
    justify-content: center;
    padding: 20px;
    width: 100px;
    margin: 3px;
    color: #fff;
    font-size: 20px;
    /* border: 3px solid gold; */
    background-color: #151b4b;
    cursor: pointer;
}

.block-question {
    margin-bottom: 10px;
}

.block-input {
    width: 100%;
}

.input-modal {
    padding: 15px;
    width: calc(50% - 17px);
}

.block-input-inner-wrapper {
    width: 100%;
}

.btn-check {
    padding: 15px;
    width: calc(50% - 17px);
}
</style>