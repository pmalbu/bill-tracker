<template>
    <table class="table">
        <thead class="bg-blue text-white">
        <tr>
            <th scope="col">Index</th>
            <th scope="col">Date</th>
            <th scope="col">Amount</th>
            <th scope="col">Category</th>
            <th scope="col">Delete</th>
        </tr>
        </thead>
        <tbody>
        <tr class="p-5 bg-blue-lighter text-center">
            <td colspan="5" class="p-5">
                <button class="underline" @click="triggerShowAddBill">Add new</button>
            </td>
        </tr>
        <tr v-for="(bill, index) in bills" :key="index" class="p-5">
            <td>{{index}}</td>
            <td>{{bill.date | moment("MMM D YYYY")}}</td>
            <td>{{bill.amount | currency }}</td>
            <td>{{bill.category}}</td>
            <td><span @click="deleteBill(index)" class="text-red">ⓧ</span></td>
        </tr>
        </tbody>
    </table>
</template>

<script>
    import Vue from 'vue'
    import Vue2Filters from 'vue2-filters'
    Vue.use(Vue2Filters); // currency filter

    export default {
        name: 'BillsTable',
        props: {
            'bills': Array,
        },
        methods: {
            triggerShowAddBill() {
                this.$emit('triggerShowAddBill')
            },
            deleteBill(index) {
                this.$emit('deleteBill', index)
            }
        },
    }
</script>