<template>
    <ul class="list-reset inline flex justify-center border-b-4 mb-0">
        <li
                class="p-4 inline bg-grey-lighter hover:bg-grey-light uppercase font-black cursor-pointer"
                v-for="category in categories"
                :key="category" >
            {{category}} <span style="color:red" @click="deleteCategory(category)">ⓧ</span>
        </li>

        <li class="p-4 inline bg-grey-lighter hover:bg-grey-light uppercase font-black cursor-pointer" @click="triggerShowAddCategory">➕</li>
    </ul>
</template>

<script>
    export default {
        name: 'NavBar',
        props: ['categories'],
        methods: {
            triggerShowAddCategory() {
                this.$emit('triggerShowAddCategory')
            },
            deleteCategory(category) {
                if (confirm('Are you sure you want to delete the "' + category + '" category?\nThis will delete all bills in this category.')) {
                    this.$emit('deleteCategory', category)
                }
            }
        }
    }
</script>