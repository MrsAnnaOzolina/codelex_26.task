<script setup lang="ts">
import { ref } from 'vue';
import HelloWorld from './WetherApp.vue';

const searchValue = ref("")
const text = ref("")
const formSubmitted = ref(false)

// const setValue = () => {
//     searchValue = text,
//     text = ref(" "
// }

</script>

<template>
    <div>
        <form class="form" @:submit.prevent="searchValue = text; text = ''; formSubmitted = true ">
            <label class="uk-form-label form--gap"> See current weather in your favorite city</label>
            <input class="form__input form--gap" type="text" placeholder="city name" v-model="text">
            <p v-if="searchValue.length === 0 && formSubmitted" style="color: red">Please write city Name </p>
            <button class="form__button form--gap"> search </button>
        </form>
        <HelloWorld :value="searchValue" />
    </div>
</template>


<style>
.form {
    margin: 50px;
    width: 40vw;
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
    border-bottom: 1px solid rgb(214, 214, 214);
    color: rgb(214, 214, 214) ;
}
.form__input:focus {
  outline: none;
  border: none;
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