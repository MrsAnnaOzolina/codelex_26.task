<script setup lang="ts">
import { ref } from 'vue';
import HelloWorld from './WetherApp.vue';

const searchValue = ref("")
const text = ref("")
</script>

<template>
    <div>
        <form class="form" @:submit.prevent="searchValue = text; text = '' ">
            <label class="uk-form-label form--gap"> Search your favorite city</label>
            <input class="form__input form--gap" type="text" placeholder="city name" v-model="text">
            <button class="form__button form--gap"> search </button>
        </form>
        <HelloWorld :value="searchValue" />
    </div>
</template>


<style>
.form {
    margin: 50px;
    width: 80vw;
    /* width: 80vw; */
    display: grid;

    color: white;
}

.form--gap {
    margin-top: 20px;
}

.form__input {
    border: none;
    background-color: transparent;
    border-bottom: 2px solid grey;
}

.form__button {
    /* background:red; */
    color: white;
    border: 0.5px solid #e7e7e7;
    height: 30px;
    width: 70px;
    border-radius: 5px;
    background-color: transparent;
}

.form__button:hover {
    background-color: #5e5757;
}
</style>