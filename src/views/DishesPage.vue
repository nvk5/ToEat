<script lang="ts">
import NewDishForm from '../components/NewDishForm.vue'
import DishCard from '../components/DishCard.vue'
import SideMenu from '../components/SideMenu.vue'
import { defineComponent } from 'vue'
import type IDish from '@/types/IDish'

interface IDataDish {
    filterText: string
    dishList: IDish[]
    showNewForm: boolean
}
export default defineComponent({
    components: {
        NewDishForm,
        DishCard,
        SideMenu,
    },
    data: (): IDataDish => ({
        filterText: '',
        dishList: [
            {
                id: '7d9f3f17-964a-4e82-98e5-ecbba4d709a1',
                name: 'Ghost Pepper Poppers',
                status: 'Want to Try',
            },
            {
                id: '5c986b74-fa02-4a22-98f2-b1ff3559e85e',
                name: 'A Little More Chowder Now',
                status: 'Recommended',
            },
            {
                id: 'c113411d-1589-414f-a283-daf7eedb631e',
                name: 'Full Laptop Battery',
                status: 'Do Not Recommend',
            },
        ],
        showNewForm: false,
    }),
    computed: {
        filteredDishList(): IDish[] {
            return this.dishList.filter((dish) => {
                if (dish.name) {
                    return dish.name.toLowerCase().includes(this.filterText.toLowerCase())
                } else {
                    return this.dishList
                }
            })
        },
        numberOfDishes(): number {
            return this.filteredDishList.length
        },
    },
    methods: {
        addDish(payload: IDish) {
            this.dishList.push(payload)
            this.hideForm()
        },
        deleteDish(payload: IDish) {
            this.dishList = this.dishList.filter((dish) => {
                return dish.id !== payload.id
            })
        },
        hideForm() {
            this.showNewForm = false
        },
    },
    mounted() {
        const route = this.$route
        if (route.query.new) {
            this.showNewForm = true
        }
    },
})
</script>

<template>
    <main class="section">
        <div class="columns">
            <!-- Side Menu -->
            <SideMenu />

            <!-- Main Content -->
            <div class="column">
                <h1 class="title">Dishes</h1>

                <!-- CTA Bar -->
                <nav v-if="!showNewForm" class="level">
                    <div class="level-left">
                        <div class="level-item">
                            <p class="subtitle is-5">
                                <strong>{{ numberOfDishes }}</strong> dishes
                            </p>
                        </div>

                        <p class="level-item">
                            <button @click="showNewForm = true" class="button is-success">New</button>
                        </p>

                        <div class="level-item is-hidden-tablet-only">
                            <div class="field has-addons">
                                <p class="control">
                                    <input class="input" type="text" placeholder="Dish name" v-model="filterText" />
                                </p>
                                <p class="control">
                                    <button class="button">Search</button>
                                </p>
                            </div>
                        </div>
                    </div>
                </nav>

                <!-- New Dish Form -->
                <NewDishForm v-if="showNewForm" @add-new-dish="addDish" @cancel-new-dish="hideForm" />

                <!-- Display Results -->
                <div v-else class="columns is-multiline">
                    <div v-for="item in filteredDishList" class="column is-full" :key="`item-${item}`">
                        <DishCard :dish="item" @delete-dish="deleteDish" />
                    </div>
                </div>
            </div>
        </div>
    </main>
</template>

<style></style>
