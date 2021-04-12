<template>
  <q-layout>
    <q-header elevated class="bg-white">
      <q-toolbar class="q-my-sm q-px-md text-dark">
        <q-avatar class="q-mr-sm">
          <img src="~assets/logo.png" alt="">
        </q-avatar>
        <div class="text-weight-bolder text-h6">MP Food</div>

        <div v-if="screen.gt.sm" class="GL__toolbar-link q-ml-xs q-gutter-md text-body2 text-weight-bold row items-center no-wrap">
          <router-link to="/" class="text-dark">
            Home
          </router-link>
          <router-link to="foods" class="text-dark">
            Foods
          </router-link>
        </div>

        <q-space />

        <div class="q-pl-sm q-gutter-sm row items-center no-wrap">
          <div>Cart</div>
          <q-btn v-if="screen.gt.xs" dense flat round size="sm" icon="fas fa-shopping-cart" />
          <q-badge color="green">
            0
          </q-badge>
        </div>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { fabGithub } from '@quasar/extras/fontawesome-v5'
const stringOptions = [
  'quasarframework/quasar',
  'quasarframework/quasar-awesome'
]
export default {
  name: 'MyLayout',
  data () {
    return {
      text: '',
      options: null,
      filteredOptions: [],
      screen: this.$q.screen
    }
  },
  methods: {
    filter (val, update) {
      if (this.options === null) {
        // load data
        setTimeout(() => {
          this.options = stringOptions
          this.$refs.search.filter('')
        }, 2000)
        update()
        return
      }
      if (val === '') {
        update(() => {
          this.filteredOptions = this.options.map(op => ({ label: op }))
        })
        return
      }
      update(() => {
        this.filteredOptions = [
          {
            label: val,
            type: 'In this repository'
          },
          {
            label: val,
            type: 'All GitHub'
          },
          ...this.options
            .filter(op => op.toLowerCase().includes(val.toLowerCase()))
            .map(op => ({ label: op }))
        ]
      })
    }
  },
  created () {
    this.fabGithub = fabGithub
  }
}
</script>

<style lang="sass">
.GL
  &__select-GL__menu-link
    .default-type
      visibility: hidden
    &:hover
      background: #0366d6
      color: white
      .q-item__section--side
        color: white
      .default-type
        visibility: visible
  &__toolbar-link
    a
      color: white
      text-decoration: none
      &:hover
        opacity: 0.7
  &__menu-link:hover
    background: #0366d6
    color: white
  &__menu-link-signed-in,
  &__menu-link-status
    &:hover
      & > div
        background: white !important
  &__menu-link-status
    color: $blue-grey-6
    &:hover
      color: $light-blue-9
  &__toolbar-select.q-field--focused
    width: 450px !important
    .q-field__append
      display: none
</style>