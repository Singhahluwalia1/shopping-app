<template>

    <div class="row m-2">
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
                <div v-for="(lesson, index) of sortedLessons" :key="lesson.id" class="lesson-card col m-3 p-3 border">
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
                        <button class="btn add-to-card" @click="add(index)">Add to card</button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div class="row m-2">
        <h2 class="text-center m-3">Shopping Cart</h2>
        <div class="col">
            <div class="lesson-list d-flex flex-wrap">
                <div v-for="(lesson, index) of slessons" :key="lesson.id" class="lesson-card col m-3 p-3 border">
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
                        <button class="btn add-to-card" @click="">Add to card</button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div class="row m-2">
        <h2 class="text-center">Checkout</h2>
        <div class="d-flex justify-content-center">
            Name:
            <label class="form-text-label ms-2">
                <input class="form-text-input" type="text" name="order" />
            </label>
            <label class="form-text-label ms-2">
                Phone
                <input class="form-text-input" type="text" name="order" />
            </label>
            <Button class="btn btn-primary ms-2">Checkout</Button>
        </div>
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
            sortby: "price",
            orderby: "asc",
        };
    },
    methods: {
        add(id) {
            if (this.lessons[id].spaces > 1) {
                let { ...lesson } = this.lessons[id]
                this.lessons[id].spaces--
                lesson.spaces = 1
                this.slessons.push(lesson)
            }
        }
    },
    computed: {
        sortedLessons() {
            if (this.lessons.length < 1) return this.lessons;

            let sortBy = this.sortBy ?? "subject";
            let ord = "desc";

            let slessons = this.lessons;
            slessons.sort(function (a, b) {
                let aa = ord === "asc" ? a : b;
                let bb = ord === "asc" ? b : a;
                if (typeof aa[sortBy] === "number") {
                    return (aa[sortBy] - bb[sortBy]);
                } else {
                    return ((aa[sortBy] < bb[sortBy]) ? -1 : ((aa[sortBy] > bb[sortBy]) ? 1 : 0));
                }
                return aa.price - bb.price;
            });
            return slessons;
        },
    },
};

</script>

<style>
.lesson-card {
    width: clamp(10rem, fit-content, 15rem);
    border-radius: 1rem;
}

.add-to-card {
    width: fit-content;
    background-color: #5590ed;
    border-color: #5590ed;
    border-radius: 2px;
    color: white;
    padding: 0 1rem;
}
</style>
