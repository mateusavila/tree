<script setup>
defineProps({
  links: {
    type: Array,
    required: true
  }
})

const toggleCollapse = (item) => {
  if (item.children) {
    item.collapse = !item.collapse
  }
}
</script>

<template>
  <ul v-if="Array.isArray(links)">
    <li v-for="(item, index) in links"
      :key="index">
      <span @click="toggleCollapse(item)"
        class="item-center">
        <img
          :src="item.children && item.children.length > 0 ? '/folder.svg' : '/link.svg'"
          alt="pasta"
          width="12"
          height="12">
        {{ item.name }}</span>
      <link-tree-preview :links="item.children"
        v-if="item.children && item.children.length > 0 && !item.collapse" />
    </li>
  </ul>
</template>

<style>
.item-center {
  font-size: 12px;
  padding: 3px;
  display: flex;
  width: 100%;
  gap: 5px;
  align-items: center;
}
</style>