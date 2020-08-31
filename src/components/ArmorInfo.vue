<template>
  <div id="armor-info" :class="{ empty: !armor }">
    <template v-if="armor">
      <img :src="armor.sprite" alt="" aria-hidden="true" draggable="true">
      <h2>{{ armor.name }}</h2>
      <div id="stats">
        <div id="defense">
          <Shirt />
          <span class="num" aria-label="current defense">
            {{ armor.defense }}
          </span>
          <span aria-hidden="true"><CaretRight /></span>
          <span class="num" aria-label="next defense">
            {{ armor.nextDefense }}
          </span>
        </div>
        <div
          class="stars"
          :aria-label="`${armor.level} ${armor.level == 1 ? 'star' : 'stars'}`"
        >
          <button
            type="button"
            aria-label="decrease level"
            :disabled="!(armor.level > 0)"
            @click="decrease"
          ><Minus /></button>
          <span
            v-for="i in 4"
            :key="i"
            :class="{ filled: i <= armor.level }"
            aria-hidden="true"
          ><Star /></span>
          <button
            type="button"
            aria-label="increase level"
            :disabled="!(armor.level < 4)"
            @click="increase"
          ><Plus /></button>
        </div>
      </div>
      <div class="upgrade-item">
        <img :src="armor.sprite" alt="" aria-hidden="true">
        <span>{{ armor.name }}</span>
        <span>1</span>
      </div>
      <div
        v-for="([ item, count ]) in armor.nextItems"
        class="upgrade-item"
        :key="item"
      >
        <img
          :src="itemSprite(item)"
          aria-hidden="true"
          width="144"
          height="144"
          alt=""
        >
        <span>{{ itemName(item) }}</span>
        <span>{{ count }}</span>
      </div>
    </template>
    <template v-else>
      <h2>No armor selected.</h2>
    </template>
  </div>
</template>

<script>
import Shirt from '@/assets/icons/shirt.svg';
import Star from '@/assets/icons/star-solid.svg';
import Plus from '@/assets/icons/plus-solid.svg';
import Minus from '@/assets/icons/minus-solid.svg';
import CaretRight from '@/assets/icons/caret-right-solid.svg';

import { userProgress, levelUp, levelDown } from '@/store';
import items from '@/assets/data/items.json';

export default {
  name: 'ArmorInfo',
  props: {
    armor: { type: Object, required: false }
  },
  components: { Shirt, Plus, Minus, CaretRight, Star },
  methods: {
    increase: function() {
      if (this.armor.level < 4) levelUp(this.armor);
    },
    decrease: function() {
      if (this.armor.level > 0) levelDown(this.armor);
    },
    itemName: function(item) {
      return items.find(i => i.tag == item).name;
    },
    itemSprite: function(item) {
      return `/images/items/${item}.png`;
    }
  }
}
</script>

<style scoped>
img {
  display: block;
}

#armor-info>* {
  margin-bottom: 0;
}

#armor-info>*+:not(h2) {
  margin-top: 0.85rem;
}

#armor-info>img {
  margin-top: 0;
}

h2 {
  margin-top: 0.5rem;
  padding-bottom: 0.85rem;
  padding-left: 0.35em;
  border-bottom: 0.1rem solid var(--block-border);
}

#stats {
  display: flex;
  flex-flow: row nowrap;
  justify-content: space-between;
  align-items: center;
}

.stars {
  display: flex;
  align-items: center;
}

.stars span {
  margin: 0.1rem;
  color: var(--body-text-t2);
}

.stars span:first-of-type {
  margin-left: 1rem;
}

.stars span:last-of-type {
  margin-right: 1rem;
}

.stars span.filled, .stars button {
  color: var(--body-text);
}

.stars button:disabled {
  color: var(--body-text-t2);
  cursor: default;
}

#defense {
  font-size: 0.85rem;
  display: flex;
  flex-flow: row nowrap;
  align-items: center;
}

#defense span {
  font-size: 1.25rem;
}

#defense>* {
  margin: 0 0.3rem;
}

svg {
  display: block;
  fill: currentColor;
  height: 1.2rem;
}

#armor-info>.upgrade-item:nth-of-type(2) {
  margin-top: 1.5rem;
}

.upgrade-item {
  display: flex;
  flex-flow: row nowrap;
  align-items: center;
  justify-content: flex-start;
  font-size: 1.35em;
  font-weight: bold;
  height: 2.2rem;
  background-color: var(--block-color-a);
  padding: 0 1em 0.15em 0.5em;
}

.upgrade-item>* {
  margin-left: 1rem;
  margin-right: 1rem;
}

.upgrade-item>:first-child {
  margin-left: 0;
}

.upgrade-item>:last-child {
  margin-right: 0;
}

.upgrade-item img {
  display: inline-block;
  align-self: flex-end;
  height: 135%;
  width: auto;
}

.upgrade-item :last-child {
  margin-left: auto;
}

#armor-info.empty h2 {
  border: none;
  color: var(--body-text-t);
  text-align: center;
  margin-top: 5rem;
  margin-bottom: 4.5rem;
}
</style>