<template>
  <div>
    <div v-for="(v, index) in $v.people.$each.$iter">
      <div class="form-group" :class="{ 'form-group--error': v.$error }">
        <label class="form__label">Name for {{index}}</label>

        <input class="form__input" v-model.trim="v.name.$model"/>

      </div>
      <div class="error" v-if="!v.name.required">Name is required.</div>
      <div class="error" v-if="!v.name.minLength">Name must have at least {{v.name.$params.minLength.min}} letters.
      </div>
    </div>

    <div>
      <button class="button" @click="people.push({name: ''})">Add</button>
      <button class="button" @click="people.pop()">Remove</button>
    </div>

    <div class="form-group" :class="{ 'form-group--error': $v.people.$error }"></div>
    <div class="error" v-if="!$v.people.minLength">List must have at least {{$v.people.$params.minLength.min}}
      elements.
    </div>
    <div class="error" v-else-if="!$v.people.required">List must not be empty.</div>
    <div class="error" v-else-if="$v.people.$error">List is invalid.</div>
    <button class="button" @click="$v.people.$touch">$touch</button>
    <button class="button" @click="$v.people.$reset">$reset</button>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import {required, minLength} from 'vuelidate/lib/validators'

export default Vue.extend({
  data: () => ({
    people: [
      {name: 'John'},
      {name: ''}
    ]
  }),
  validations: {
    people: {
      required,                     //это валидаторы к состоянию массива people.
      minLength: minLength(3),  //минимальный размер листа people. Минимум 3 члена в массиве.
      $each: {
        name: {
          required,
          minLength: minLength(2)
        }
      }
    }
  }
});
</script>

<style scoped lang="scss">
</style>
