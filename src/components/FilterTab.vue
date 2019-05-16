<template>
  <div
    @click="changeTabandApplyFilter"
    class="filter__item"
    :class="{tabStyle: checkIfSelectedTab}"
  >{{tab}}</div>
</template>

<script>
export default {
  name: "FilterTab",
  methods: {
    changeTabandApplyFilter() {
      this.$emit("change-tab-and-filter", this.id);
    }
  },

  props: {
    tab: {
      required: true,
      type: String
    },
    id: {
      required: true,
      type: Number
    },
    selectedTab: {
      required: true,
      type: Number
    }
  },
  computed: {
    checkIfSelectedTab() {
      const result = this.selectedTab === this.id ? true : false;
      return result;
    }
  }
};
</script>

<style scoped>
.filter__item {
  font-size: 2.4rem;
  padding: 1rem;
  margin-right: 1rem;
  border: 2px solid #03a9f4;
  cursor: pointer;
  font-family: "Roboto", sans-serif;
  user-select: none;
  -webkit-touch-callout: none;
}

.tabStyle {
  background-color: #03a9f4;
  color: white;
  border: none;
}
</style>


