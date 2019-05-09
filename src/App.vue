<template>
    <div id="app">
        <AddCategory v-if="shouldShowAddCategory"
                     v-on:addCategory="addCategory"
                     v-on:cancelAddCategory="cancelAddCategory"/>
        <div v-else>
            <AddBill v-if="shouldShowAddBill"
                     :categories="categories"
                     v-on:addBill="addBill"
                     v-on:cancelAddBill="cancelAddBill"/>
            <div v-else>
                <NavBar :categories=categories
                        v-on:triggerShowAddCategory=triggerShowAddCategory
                        v-on:deleteCategory=deleteCategory />
                <div class="container flex">
                    <div class="w-1/2 bg-grey-lighter">
                        <BillsTable :bills="bills"
                                    v-on:triggerShowAddBill="triggerShowAddBill"
                                    v-on:deleteBill="deleteBill"/>
                    </div>
                    <div class="w-1/2 bg-grey-light pt-4 pl-4 text-2xl">
                        <Chart :bills="bills"/>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import Vue from 'vue'

    import AddCategory from './components/AddCategory'
    import AddBill from './components/AddBill'
    import Chart from './components/Chart'
    import BillsTable from './components/BillsTable'
    import NavBar from './components/NavBar'

    Vue.use(require('vue-moment'));

    export default {
        name: 'app',
        components: {
            AddCategory,
            AddBill,
            Chart,
            BillsTable,
            NavBar,
        },
        data () {
            return {
                bills: [],
                categories: [],
                shouldShowAddCategory: false,
                shouldShowAddBill: false,
            }
        },
        methods: {
            ////////////////
            // Categories //
            ////////////////
            addCategory (category) {
                if (this.categories.indexOf(category) !== -1) {
                    alert('You have already added this category!')
                } else {
                    this.categories.push(category);
                    this.shouldShowAddCategory = false
                }
            },
            cancelAddCategory () {
                this.shouldShowAddCategory = false
            },
            deleteCategory (categoryToDelete) {
                this.bills = this.bills.filter(bill => bill.category !== categoryToDelete)
                this.categories.splice(this.categories.indexOf(categoryToDelete), 1)
            },
            triggerShowAddCategory () {
                this.shouldShowAddCategory = true
            },

            ////////////////
            // Bills      //
            ////////////////
            addBill (bill) {
                this.bills.push(bill);
                this.shouldShowAddBill = false
            },
            deleteBill (index) {
                this.bills.splice(index, 1)
            },
            cancelAddBill () {
                this.shouldShowAddBill = false
            },
            triggerShowAddBill () {
                this.shouldShowAddBill = true
            },
        },
        watch: {
            categories () {
                localStorage.setItem('categories', JSON.stringify(this.categories))
            },
            bills () {
                localStorage.setItem('bills', JSON.stringify(this.bills))
            }
        },
        mounted () {
            if (localStorage.getItem('categories')) {
                this.categories = JSON.parse(localStorage.getItem('categories'))
            }

            if (localStorage.getItem('bills')) {
                this.bills = JSON.parse(localStorage.getItem('bills'))
            }

            if (!this.categories.length) {
                this.shouldShowAddCategory = true
            }

            if (!this.bills.length) {
                this.shouldShowAddBill = true
            }
        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
</style>
