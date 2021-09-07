<template>
  <div>
    <div class="tab-bar">
      <button
        v-for="(tab, i) in tabs"
        :key="i"
        @click.self="selectTab(i)"
        :class="{ selected: selectedTab == i }"
      >
        <div @click.self="selectTab(i)">{{ tab.title }}</div>
        <div v-show="this.dynamic" class="close" @click="deleteTab(i)">
          &#x1F7A9;
        </div>
      </button>

      <button v-show="this.dynamic" @click="createTab()" class="add">+</button>
    </div>

    <div
      v-for="(tab, i) in tabs"
      :key="i"
      v-show="selectedTab == i"
      class="tab"
    >
      <slot v-if="this.dynamic"></slot>
      <slot v-else :name="`tab-${i + 1}`"></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "TabSystem",
  props: {
    dynamic: {
      type: Boolean,
      required: false,
      default: true,
    },
    defaultTitle: {
      type: String,
      required: false,
      default: "New Tab",
    },
    injectedTabs: {
      type: Array,
      required: false,
      default: (props) => [
        {
          title: props.defaultTitle,
        },
      ],
    },
  },
  data() {
    return {
      selectedTab: 0,
      tabs: [...this.injectedTabs],
    };
  },
  methods: {
    selectTab(id) {
      this.selectedTab = id;
    },
    createTab() {
      this.tabs.push({
        title: this.defaultTitle,
      });
      this.selectTab(this.tabs.length - 1);
    },
    deleteTab(id) {
      this.tabs.splice(id, 1);
      if (this.selectedTab >= this.tabs.length) {
        this.selectTab(this.tabs.length - 1);
      }
    },
  },
};
</script>

<style lang="sass" scoped>
button
  border: none
  border-radius: 6px
  background-color: #dedede
  font-size: .8rem
  height: 2rem
  max-width: 10rem
  position: relative
  display: inline-flex
  justify-content: space-between
  align-items: center
  cursor: pointer

  div
    white-space: nowrap

  &:not(.add)
    flex: 1 10 2rem
    text-align: left
    padding: 0 .4rem 0 .6rem
    overflow: clip

  &:hover, &:focus-visible
    &:not(.selected)
      background-color: #ededed

      .close
        box-shadow: 20px 0 20px 50px #ededed
        background-color: #ededed

  .close
    display: inline
    padding: 0px 3px
    border-radius: 6px
    box-shadow: 20px 0 20px 50px #dedede
    background-color: #dedede
    font-size: 1.2rem

    &:hover, &:focus-visible
      background-color: #ffffffbb !important

.tab-bar
  display: flex
  flex-wrap: wrap
  overflow-x: hidden
  gap: 5px
  padding: 5px 0

.add
  justify-content: center
  background-color: transparent
  font-size: 1.5rem
  color: #aaa
  width: 2rem
  height: 2rem
  margin-left: auto

.selected
  background-color: lightblue

  .close
    box-shadow: 20px 0 20px 50px lightblue
    background-color: lightblue

button.selected
  &:hover, &:focus-visible
    background-color: #BDE8F6

    .close
      box-shadow: 20px 0 20px 50px #BDE8F6
      background-color: #BDE8F6

.tab
  padding: 2rem
  border-radius: 6px
  border: 1px solid #dedede
</style>
