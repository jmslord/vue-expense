<template>
    <div>
        <h3>History</h3>
        <ul id="list" class="list">
            <li v-for="transaction in transactions" :key="transaction.id" :class="transaction.amount < 0 ? 'minus' : 'plus'">
                {{ transaction.text }} <span>${{ transaction.amount }}</span>
                <button class="delete-btn" @click="deleteTransaction(transaction.id)">x</button>
            </li>
        </ul>
    </div>
</template>

<script setup>
import {defineProps} from 'vue'

const props = defineProps({
    transactions: {
        type: Array,
        required: true
    }
})

const emit = defineEmits(['delete-transaction'])
const deleteTransaction = (id) => {
    emit('delete-transaction', id)
}
</script>

<!-- <script>
export default {
    data() {
        return {
            transactions: [
                { id: 1, text: 'Flower', amount: -20 },
                { id: 2, text: 'Salary', amount: 300 },
                { id: 3, text: 'Book', amount: -10 },
                { id: 4, text: 'Camera', amount: 150 }
            ]
        }
    }
}
</script> -->