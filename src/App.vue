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
         </div>
    </div>

  
</template>

<script>

import lessons from "./data/lessons";
import "./assets/fontawesome.css";

export default {
    name: "App",
    
    
};

</script>

<style>

</style>
