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
        <tr>
            <td colspan="4">
                <button class="underline" @click="triggerShowAddBill">Add new</button>
            </td>
        </tr>
        <tr v-for="(bill, index) in sortedBills" :key="index" class="p-4">
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

    Array.prototype.indexOfBill = function (bill) {
        for (var i = 0, len = this.length; i < len; i++) {
            if (this[i]['date'] === bill.date &&
                this[i]['amount'] === bill.amount &&
                this[i]['category'] === bill.category) {
                return i;
            }
        }
        return -1;
    };

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
                let bill = this.sortedBills[index];
                let indexToDelete = this.bills.indexOfBill(bill);
                this.$emit('deleteBill', indexToDelete)
            }
        },
        computed: {
            sortedBills() {
                // use slice() to create a copy of the array
                return this.bills.slice().sort((a, b) => new Date(a.date) - new Date(b.date));
            },

        },
    }
</script>