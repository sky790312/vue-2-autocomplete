<template>
  <div class="dropdown" :class="{'open': isDropdownMenuOpen}">
      <input class="form-control" type="text" v-model="model.value"
          @keydown.enter = "enter"
          @keydown.down = "down"
          @keydown.up = "up"
          @input = "change"/>
      <ul class="dropdown-menu">
          <li v-for="(match, index) in matches"
              :class="['item', {'active': isActive(index)}]"
              @click="selected(index)">
              <a>{{ match }}</a>
          </li>
      </ul>
  </div>
</template>

<script>
export default {
  data () {
    return {
      isOpen: false,
      current: 0
    }
  },

  props: {
    schema: {
      type: Array
    },
    model: {
      type: Object
    }
  },

  computed: {
    matches () {
      return this.schema.filter((str) => {
        return str.indexOf(this.model.value) >= 0
      })
    },

    isDropdownMenuOpen () {
      return this.model.value && this.matches.length && this.isOpen
    }
  },

  methods: {
    enter () {
      this.model.value = this.matches[this.current]
      this.isOpen = false
    },

    up () {
      if (this.current > 0) this.current--
    },

    down () {
      if (this.current < this.schema.length - 1) this.current++
    },

    isActive (index) {
      return index === this.current
    },

    change () {
      if (!this.isOpen) {
        if (this.matches.length <= 1) this.current = 0
        this.isOpen = true
      }
    },

    selected (index) {
      this.model.value = this.matches[index]
      this.isOpen = false
    }
  }
}
</script>

<style lang="stylus">
.dropdown {
  position: relative

  > input {
    width: 100%
  }

  &.open {
    > .dropdown-menu {
      display: block
    }
  }

  .dropdown-menu {
    width: 100%
    position: absolute
    top: 100%
    left: 0
    z-index: 1000
    display: none
    float: left
    min-width: 10rem
    padding: .5rem 0
    margin: .125rem 0 0
    font-size: 1rem
    color: #373a3c
    text-align: left
    list-style: none
    background-color: #fff
    -webkit-background-clip: padding-box
    background-clip: padding-box
    border: 1px solid rgba(0,0,0,.15)
    border-radius: .25rem

    .item {
      color: #45b29d
      cursor: pointer
      margin-left: 10px
      &:hover, &:focus {
        color: #307d6e
      }
    }

    .active {
      opacity: .8
      background-color: gray
    }
  }
}
</style>
