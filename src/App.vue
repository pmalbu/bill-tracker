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
                <NavBar :categories=categories :activeCategory=activeCategory
                        v-on:setActiveCategory=setActiveCategory
                        v-on:clearActiveCategory=clearActiveCategory
                        v-on:triggerShowAddCategory=triggerShowAddCategory
                        v-on:deleteCategory=deleteCategory />
                <div class="container flex">
                    <div class="w-1/2 bg-grey-lighter">
                        <BillsTable :bills="activeBills"
                                    v-on:triggerShowAddBill="triggerShowAddBill"
                                    v-on:deleteBill="deleteBill"/>
                    </div>
                    <div class="w-1/2 bg-grey-light pt-4 pl-4 text-2xl">
                        <Chart :bills="activeBills" />
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

    Array.prototype.indexOfBill = function (bill) {
        // console.log('looking for: ' + bill.date + ' ' + bill.amount + ' ' + bill.category)
        for (var i = 0, len = this.length; i < len; i++) {
            // console.log(this[i]['date'] + ' ' + this[i]['amount'] + ' ' + this[i]['category'])
            if (this[i]['date'] === bill.date &&
                this[i]['amount'] === bill.amount &&
                this[i]['category'] === bill.category) {
                return i;
            }
        }
        return -1;
    };

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
                activeCategory: '',
                generateValues: false,
            }
        },
        computed: {
            activeBills() {
                return this.bills
                    .filter(
                        bill =>
                            this.activeCategory ? bill.category === this.activeCategory : true
                    )
                    .sort((a, b) => (new Date(a.date) < new Date(b.date) ? 1 : -1))
            }
        },
        methods: {
            ////////////////
            // Categories //
            ////////////////
            addCategory (category) {

                if (this.categories.indexOf(category) !== -1) {
                    alert('You have already added this category!')
                }
                else {
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

            // Filter bills based on category
            setActiveCategory(category) {
                this.activeCategory = category
            },

            clearActiveCategory() {
                this.activeCategory = ''
            },

            ////////////////
            // Bills      //
            ////////////////
            addBill (bill) {
                this.bills.push(bill);
                this.shouldShowAddBill = false
            },
            deleteBill (index) {

                let bill = this.activeBills[index];
                let indexToDelete = this.bills.indexOfBill(bill);
                this.bills.splice(indexToDelete, 1)
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

            var months = [6,7,8,9,10,11,12,1,2,3,4,5]
            let dates = [];
            months.forEach((month) => {
                let year = 2018;
                if (month < 6) {
                    year = 2019;
                }

                var min=1;
                var max=28;
                var day = Math.random() * (+max - +min) + +min;

                dates.push(new Date(year, month, day));
            });

            if (this.generateValues) {

                this.addCategory('Internet')
                this.addCategory('Gas')
                this.addCategory('Electricity')

                this.categories.forEach((cat, i) => {
                    dates.forEach(day => {

                        var min = 20;
                        var max = 60;
                        var amt = Math.random() * (+max - +min) + +min;

                        let b = {
                            date: day,
                            amount: amt,
                            category: cat,
                        }

                        this.addBill(b)
                    });
                });
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
