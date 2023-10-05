<script setup>
import nestedDraggable from "./nested-draggable.vue"

defineProps({
  list: {
    type: Array,
    required: true
  }
})

const emit = defineEmits(['addLink', 'edit', 'urlText'])
const addLink = () => emit('addLink')
const edit = (data) => emit('edit', data)
const urlText = (data) => emit('urlText', data)
</script>

<template>
  <div class="link-tree">
    <nested-draggable :tasks="list"
      @edit="edit"
      @urlText="urlText" />
    <button @click="addLink"
      type="button"
      class="button-add">Adicionar link</button>
  </div>
</template>

<style>
.link-tree {
  width: calc(50% - 10px);
}

.button-add {
  margin-top: 10px;
  border: none;
  background: #787878;
  color: #fff;
  font-weight: 100;
  cursor: pointer;
  font-size: 14px;
  padding: 8px 12px;
  border-radius: 6px;
  transition: all .2s ease-in-out;
}
</style>

