<template>
  <div style="text-align: center;">
    <p> Enter a repository name: </p>
    <v-autocomplete :items="items" v-model='item' :get-label='getLabel' :min-len='0' @update-items='update' :component-item='tpl' @item-selected="itemSelected" @item-clicked="itemClicked" :input-attrs="{name: 'input-test', id: 'v-my-autocomplete'}">
    </v-autocomplete>
    <p> Selected item: </p>
    <pre> {{ item }} </pre>
  </div>
</template>

<script>
import Autocomplete from 'v-autocomplete'
import tpl from './components/TplItem.vue'
import axios from 'axios'

export default {
  name: 'app',
  components: {'v-autocomplete': Autocomplete},
  data () {
    return {
      itemsApi: [],
      item: { name: "First element" },
      items: [],
      tpl: tpl
    }
  },
  methods: {
    itemSelected (item) {
      console.log('Selected item!', item)
    },
    itemClicked (item) {
      console.log('You clicked an item!', item)
    },
    getLabel (item) {
      if (item) {
        return item.name
      }
      return ''
    },
    update (text) {
      if(text != ''){
        axios.get('https://api.github.com/search/repositories?q=' + text)
        .then(response => {
            this.itemsApi = response.data.items
            this.items = this.itemsApi.filter((item) => {
              return (new RegExp(text.toLowerCase())).test(item.name.toLowerCase())
            })
          }
        )
        .catch(e => {
          return false
        })
      }
    }
  }
}
</script>

<style src="./assets/css/style.css"></style>
<style lang="stylus">
.v-autocomplete
  .v-autocomplete-input-group
    .v-autocomplete-input
      font-size 1.5em
      padding 10px 15px
      box-shadow none
      border 1px solid #157977
      width calc(100% - 32px)
      outline none
      background-color #eee
    &.v-autocomplete-selected
      .v-autocomplete-input
        color green
        background-color #f2fff2
  .v-autocomplete-list
    width 100%
    text-align left
    border none
    border-top none
    max-height 400px
    overflow-y auto
    border-bottom 1px solid #157977
    .v-autocomplete-list-item
      cursor pointer
      background-color #fff
      padding 10px
      border-bottom 1px solid #157977
      border-left 1px solid #157977
      border-right 1px solid #157977
      &:last-child
        border-bottom none
      &:hover
        background-color #eee
      abbr
        opacity 0.8
        font-size 0.8em
        display block
        font-family sans-serif

pre
  text-align left
  white-space pre-wrap
  background-color #eee
  border 1px solid silver
  padding 20px !important
  border-radius 10px
  font-family monospace !important
.left
  text-align left
.note
  border-left 5px solid #ccc
  padding 10px
</style>
