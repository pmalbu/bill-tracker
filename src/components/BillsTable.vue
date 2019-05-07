<template>
    <table class="table">
        <thead class="bg-blue text-white">
        <tr>
            <th scope="col">Date</th>
            <th scope="col">Amount</th>
            <th scope="col">Category</th>
            <th scope="col">Delete</th>
        </tr>
        </thead>
        <tbody>
        <tr>
            <td colspan="4">
                <button class="underline" @click="triggerShowAddBill">Add new</button>
            </td>
        </tr>
        <tr v-for="(bill, index) in bills" :key="index" class="p-4">
            <td>{{bill.date | moment("MMM D YYYY")}}</td>
            <td>${{bill.amount}}</td>
            <td>{{bill.category}}</td>
            <td><span @click="deleteBill(index)" class="text-red">ⓧ</span></td>
        </tr>
        </tbody>
    </table>
</template>

<script>
    export default {
        name: 'BillsTable',
        props: ['bills'],
        data () {
            return {}
        },
        methods: {
            triggerShowAddBill() {
                this.$emit('triggerShowAddBill')
            },
            deleteBill(index) {
                this.$emit('deleteBill', index)
            }
        }
    }
</script>