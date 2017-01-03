<template>
  <div class="auto-select">
    <div class="auto-select-show" :class="{editing: editing}">
      <ul class="auto-select-selecteds" v-on:click.selt.stop.prevent="$refs.hint.focus()">
        <li class="auto-select-item-selected" v-for="item in selecteds"><i class="icon icon-remove" @click="remove($event, item)"></i>{{item[label.label]}}</li>
        <li><input class="auto-select-hint" type="text"
          :style="hint_width"
          @focus="editing = true"
          @blur="editing == false"
          @keydown.delete="remove_hint"
          @keydown.up.prevent="hint_up"
          @keydown.down.prevent="hint_down"
          @keydown.enter.prevent="select($event, filter_items[index])"
          v-model="hint"
          ref="hint"
          /></li>
      </ul>
    </div>
    <ul class="auto-select-list" v-show="editing" ref="list">
      <li class="auto-select-item" :class="{active: i===index, selected: selecteds.indexOf(item) >= 0}" ref="items" v-for="(item, i) in filter_items" @mouseenter="index = i" @mousedown="select($event, item)">{{item[label.label]}}</li>
    </ul>
  </div>
</template>
<script>
  export default {
    name: 'smart-select',
    data () {
      return {
        hint: '',
        editing: false,
        index: 0
      }
    },
    computed: {
      hint_width: function () {
        return {width: 0.75 + this.hint.length + 'rem'}
      },
      filter_items: function () {
        this.hint = this.hint || ''
        var aims = this.hint.split(/\s+/)
        var len = aims.length
        this.index = 0
        if (this.hint || len > 1) {
          return this.items.filter((item) => {
            for (var i = 0; i < len; i++) {
              var aim = aims[i]
              if (!((item[this.label.label] && item[this.label.label].match(aim)) || (item[this.label.value] && item[this.label.value].match(aim)))) {
                return false
              }
            }
            return true
          })
        } else {
          return this.items
        }
      }
    },
    props: {
      items: {
        type: Array,
        require: true
      },
      selecteds: {
        default: []
      },
      multiple: {
        default: false
      },
      repeat: {
        default: false
      },
      label: {
        type: Object,
        default: function () {
          return {label: 'label', value: 'value'}
        }
      }
    },
    methods: {
      reset_scroll() {
        var item = this.$refs.items[this.index]
        var bottomOverflowDistance = item.getBoundingClientRect().bottom - this.$refs.list.getBoundingClientRect().bottom
        var topOverflowDistance = item.getBoundingClientRect().top - this.$refs.list.getBoundingClientRect().top
        if (bottomOverflowDistance > 0) {
          this.$refs.list.scrollTop += bottomOverflowDistance;
        }
        if (topOverflowDistance < 0) {
          this.$refs.list.scrollTop += topOverflowDistance;
        }
      },
      hint_up () {
        if (this.index > 0) {
          --this.index
        } else {
          this.index = this.filter_items.length - 1
        }
        this.reset_scroll()
      },
      hint_down () {
        if (this.index < this.filter_items.length - 1) {
          ++this.index
        } else {
          this.index = 0
        }
        this.reset_scroll()
      },
      remove_hint(e) {
        if (!this.hint.length) {
          var item = this.selecteds.pop()
          if (item) this.hint = item[this.label.label] + ' '
        }
      },
      select(e, item) {
        if (this.selecteds.indexOf(item) === -1) {
          this.selecteds.push(item)
          this.hint = ''
        }
      },
      remove (e, item) {
        this.selecteds.splice(this.selecteds.indexOf(item), 1)
      }
    },
    watch: {
      hint (to, from) {
        // console.log('watch', arguments);
        // TODO change computed to watch
      }
    }
  }
</script>
<style scoped>
  i.icon {
    font-style: normal;
  }
  i.icon-remove::before {
    content: '✖';
    margin-right: .5rem;
    cursor: pointer;
  }
  .auto-select {
    position: relative;
    width: 100%;
    max-width: 300px;
  }
  .auto-select-show {
    min-height: 2rem;
    background-color: #fff;
    outline: 1px #000 solid;
    cursor: text;
  }
  .auto-select-show.editing {
    outline: 2px #08e solid;
  }
  .auto-select-show li{
    float: left;
    max-width: 100%;
    margin: 3px 2px 0;
  }
  .auto-select-show .auto-select-hint {
    border: 0;
    font-size: 1.2rem;
    line-height: 1.2rem;
    max-width: 100%;
  }
  .auto-select-show .auto-select-hint:focus {
    outline: none;
    outline-offset: 0;
  }
  .auto-select-selecteds {
    display: inline-block;
    box-sizing: border-box;
    min-height: 2rem;
    width: 100%;
    margin: 0;
    padding: 4px 4px 0 4px;
    list-style-type: none;
  }
  .auto-select-selecteds .auto-select-item-selected {
    height: 1.4rem;
    line-height: 1.4rem;
    padding: 0 7px;
    background-color: #0df;
    border-radius: 5px;
    margin: 3px 2px;
    user-select: none;
  }
  .auto-select-list {
    width: 100%;
    margin: 0;
    padding: 0;
    top: 2px;
    position: relative;
    outline: 2px #08e solid;
    background-color: #fff;
    list-style-type: none;
    max-height: 24rem;
    overflow-y: auto;
  }
  .auto-select-item {
    display: list-item;
    box-sizing: border-box;
    padding-left: 2rem;
    height: 2rem;
    font-size: 1.4rem;
    line-height: 2rem;
    margin: 0;
    width: 100%;
    text-align: left;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  .auto-select-item.active {
    background-color: #0df;
  }
  .auto-select-item.selected {
    background-color: #eee;
  }
  .auto-select-item.selected.active {
    background-color: #9ff;
  }
</style>
