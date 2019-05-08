<template>
    <div class="h-100 w-full flex items-center justify-center font-sans">
        <div class="bg-white rounded shadow p-6 m-4 w-full lg:w-3/4 lg:max-w-lg">
            <div class="mb-4">
                <h1 class="text-grey-darkest">Enter a new bill</h1>
                <p></p>
                <div class="flex mt-4">
                    <datepicker wrapper-class="shadow appearance-none border rounded text-grey-darker"
                                input-class="py-2 px-3 mr-4 " v-model="date"></datepicker>

                    <select v-model="category">
                        <option disabled>--Select a category--</option>
                        <option v-for="category in categories" :value="category" :key="category">
                            {{ category }}
                        </option>
                    </select>

                    <input class="shadow appearance-none border rounded w-full py-2 px-3 mr-4 text-grey-darker"
                           placeholder="Set amount" v-model.number="amount" type="number">
                    <button class="flex-no-shrink mr-1 p-2 border-2 rounded bg-teal text-white border-teal hover:text-white hover:bg-teal"
                            @click="addBill">Add
                    </button>
                    <button class="flex-no-shrink p-2 border-2 rounded bg-white text-blue border-blue"
                            @click="cancel">Cancel
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import Datepicker from 'vuejs-datepicker'

    export default {
        name: 'AddBill',
        components: {
            Datepicker,
        },
        props: ['categories'],
        data () {
            return {
                amount: 0,
                category: null,
                date: new Date(),
            }
        },
        methods: {
            addBill () {
                if (!this.date) {
                    alert('Please enter a date')
                    return
                }

                if (!this.amount) {
                    alert('Please enter an amount')
                    return
                }

                if (!this.category) {
                    alert('Please enter a category')
                    return
                }

                this.$emit('addBill', {
                    amount: this.amount,
                    date: this.date,
                    category: this.category
                })
            },
            cancel() {
                this.$emit('cancelAddBill')
            }
        },
    }
</script>