<template>
  <div class="emoji-vue-wraper" v-bind:id="id">
    <div class="emoji-picker-container">
      <textarea cols="50" v-model="value" rows="10" data-vue-emojiable="true" class="emoji-vue-textarea"></textarea>
    </div>
  </div>
</template>

<script>
import ns from 'nanoscroller/bin/javascripts/jquery.nanoscroller'

import './lib/css/emoji.css'
import 'nanoscroller/bin/css/nanoscroller.css'

import storageUtils from './lib/js/util'
import {EmojiPicker} from './lib/js/emoji-picker'
import {getGuid} from './lib/js/jquery.emojiarea'
import {nextTick} from "vue";

import './lib/img/IconsetSmiles.png'
import './lib/img/IconsetSmiles_1x.png'
import './lib/img/IconsetW.png'
import './lib/img/IconsetW_1x.png'

export default {
  name: 'VueEmoji',
  data () {
    return {
      id: getGuid()
    }
  },
  props: {
    width: {
      type: String,
      default: '200'
    },
    height: {
      type: String,
      default: '50'
    },
    value: {
      type: String,
      default: ''
    }
  },
  watch: {
    value: function (newVal) {
      nextTick(() => {
        this.$el.querySelector('.emoji-wysiwyg-editor').innerHTML += newVal;
      })
    }
  },
  methods: {
    clear () {
      console.log('clear')
      this.$el.querySelector('.emoji-wysiwyg-editor').innerHTML = ''
    },
    initEmojiPlugin () {
      window.emojiPicker = new EmojiPicker({
        emojiable_selector: `#${this.id} [data-vue-emojiable=true]`,
        assetsPath: '',
        popupButtonClasses: 'smile-icon',
        onChangecontent: (textAreaValue) => {
          this.$emit('input', {data: textAreaValue})
        }
      })
      window.emojiPicker.discover()
      nextTick(() => {
        const editor = this.$el.querySelector('.emoji-wysiwyg-editor')
        editor.style.width = `${this.width}px`
        editor.style.height = `${this.height}px`
        this.$el.querySelector('.emoji-picker-container').style.width = `${Number(this.width) + 50}px`
      })
    }
  },
  mounted: function () {
    nextTick(() => {
      storageUtils()
      this.initEmojiPlugin()
    })
  }
}
</script>

<style>
 i.emoji-picker-icon.emoji-picker {
   position: absolute;
   top: 256px;
   left: 125px;

   width: 56px;
   height: 44px;

   box-shadow: 0px 1px 2px rgba(21, 21, 21, 0.15);
   border-radius: 12px;

   background: url('./lib/img/emoji-normal.png') no-repeat center center;
   background-color: #FFFFFF;
 }
</style>
