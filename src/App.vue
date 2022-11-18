<template>

    <div class="d-flex justify-content-center m-5">
        <button class="btn btn-info ms-3" @click="this.page = 'lessons'" :disabled="this.page == 'lessons'">Lessons
            Page</button>
        <button class="btn btn-info ms-3" @click="this.page = 'shopping'" :disabled="this.page == 'shopping'">Shopping
            Cart Page</button>
        <button class="btn btn-info ms-3" @click="this.page = 'checkout'" :disabled="this.page == 'checkout'">Checkout
            Page</button>
    </div>

    <div class="row m-2" v-if="this.page == 'lessons'">
        <div class="col-sm-2">
            <div class="row">
                <h5>Sort by</h5>
                <label class="form-check-label">
                    <input class="form-check-input" type="radio" name="sort-by" v-model="sortby" value="subject" />
                    Subject
                </label>
                <label class="form-check-label">
                    <input class="form-check-input" type="radio" name="sort-by" v-model="sortby" value="location" />
                    Location
                </label>
                <label class="form-check-label">
                    <input class="form-check-input" type="radio" name="sort-by" v-model="sortby" value="price" />
                    Price
                </label>
                <label class="form-check-label">
                    <input class="form-check-input" type="radio" name="sort-by" v-model="sortby" value="spaces" />
                    Availability
                </label>
            </div>
            <div class="row mt-5">
                <label class="form-check-label">
                    <input class="form-check-input" type="radio" name="order" v-model="orderby" value="asc" />
                    Ascending
                </label>
                <label class="form-check-label">
                    <input class="form-check-input" type="radio" name="order" v-model="orderby" value="desc" />
                    Descending
                </label>
            </div>
        </div>
        <div class="col">
            <div class="lesson-list d-flex flex-wrap">
                <div v-for="lesson of sortedLessons" :key="lesson.id" class="lesson-card col m-3 p-3">
                    <div class="row">
                        <div class="col">
                            <div>
                                <span>Subject: </span>
                                <span>{{ lesson.subject }}</span>
                            </div>
                            <div>
                                <span>Location:</span>
                                <span>{{ lesson.location }}</span>
                            </div>
                            <div>
                                <span>Price:</span>
                                <span>£{{ lesson.price }}</span>
                            </div>
                            <div>
                                <span>Spaces:</span>
                                <span>{{ lesson.spaces }}</span>
                            </div>
                        </div>

                        <div class="col"></div>
                    </div>

                    <div class="d-flex justify-content-center mt-2">
                        <button class="btn" @click="addLessonToCart(lesson.id)" :disabled="lesson.spaces < 1">Add to
                            card</button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div class="row m-2" v-if="this.page == 'shopping'">
        <h2 class="text-center m-3">Shopping Cart</h2>
        <div class="col">
            <div class="lesson-list d-flex flex-wrap">
                <div v-for="lesson of slessons" :key="lesson.id" class="lesson-card col m-3 p-3">
                    <div class="row">
                        <div class="col">
                            <div>
                                <span>Subject:</span>
                                <span>{{ lesson.subject }}</span>
                            </div>
                            <div>
                                <span>Location:</span>
                                <span>{{ lesson.location }}</span>
                            </div>
                            <div>
                                <span>Price:</span>
                                <span>£{{ lesson.price }}</span>
                            </div>
                            <div>
                                <span>Spaces:</span>
                                <span>{{ lesson.spaces }}</span>
                            </div>
                        </div>

                        <div class="col"></div>
                    </div>

                    <div class="d-flex justify-content-center mt-2">
                        <button class="btn add-to-card" @click="removeLessonFromCart(lesson.id)">Remove</button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div class="row m-2" v-if="this.page == 'checkout'">
        <h2 class="text-center">Checkout</h2>
        <form @submit.prevent="checkOut">
            <div class="d-flex justify-content-center">
                Name:
                <label class="form-text-label ms-2">
                    <input class="form-text-input" type="text" name="name" v-model="name" required />
                </label>
                <label class="form-text-label ms-2">
                    Phone
                    <input class="form-text-input" type="tel" name="phone" v-model="phone" required />
                </label>
                <Button class="btn btn-primary ms-2">Checkout</Button>
            </div>
        </form>
    </div>
</template>

<script>

import lessons from "./data/lessons";
import "./assets/fontawesome.css";

export default {
    name: "App",
    data() {
        return {
            msg: "Hellow",
            lessons: lessons,
            slessons: [],
            sortby: "subject",
            orderby: "asc",
            name: '',
            phone: '',
            page: 'lessons'
        };
    },
    methods: {
        addLessonToCart(id) {
            this.lessons.find((lesson) => {
                if (lesson.id == id && lesson.spaces > 0) {
                    let { ...newlesson } = lesson
                    lesson.spaces--
                    newlesson.spaces = 1
                    let done = this.slessons.find((slesson) => {
                        if (slesson.id == id) {
                            slesson.spaces++
                            return true
                        }
                    })
                    if (!done) {
                        this.slessons.push(newlesson)
                    }
                }
            })
        },
        removeLessonFromCart(id) {
            for (let i = 0; i < this.slessons.length; i++) {
                if (this.slessons[i].id == id) {
                    if (this.slessons[i].spaces <= 1) {
                        this.slessons.splice(i)
                    } else {
                        this.slessons[i].spaces--;
                    }
                    this.lessons.find((lesson) => {
                        if (lesson.id == id) {
                            lesson.spaces++
                            return true
                        }
                    })
                }
            }
        },
        checkOut(e) {
            let formData = new FormData(e.target)
            let name = formData.get('name')
            alert(`${name}, Your order with ${this.slessons.length} lessons placed successfully.`)
            this.slessons = []
        }
    },
    computed: {
        sortedLessons() {
            if (this.lessons.length < 1) return this.lessons;

            let sortBy = this.sortby ?? "subject";
            let ord = this.orderby ?? "asc";

            return this.lessons.sort(function (a, b) {
                let newa = ord === "asc" ? a : b;
                let newb = ord === "asc" ? b : a;
                if (typeof newa[sortBy] === "number") {
                    return (newa[sortBy] - newb[sortBy]);
                } else {
                    return ((newa[sortBy] < newb[sortBy]) ? -1 : ((newa[sortBy] > newb[sortBy]) ? 1 : 0));
                }
            });
        },
    },
};

</script>

<style scoped>
body {
    background: #e1f8f4;
}

.lesson-card {
    display: flex;
    justify-content: space-between;
    flex-direction: column;
    border: 2px solid grey;
    width: 20rem;
    border-radius: 1rem;
}

button:hover,
button:active,
.lesson-card:hover,
.lesson-card:active {
    outline: 2px solid red;
    outline-offset: 2px;
}

.lesson-card button {
    width: fit-content;
    background: green !important;
    border-color: #5590ed;
    border-radius: 2px;
    color: white !important;
    padding: 0 1rem;
}
</style>
