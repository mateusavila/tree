<script setup>
import draggable from "vuedraggable"
defineProps({
  tasks: {
    required: true,
    type: Array
  }
})
const emit = defineEmits(['edit', 'urlText'])

const edit = () => {
  const id = +event.target.dataset.id
  const newName = event.target.innerText
  if (newName.length) {
    emit('edit', { id, newName })
  }
}
const urlText = () => {
  const id = +event.target.dataset.id
  const url = event.target.innerText
  if (url.length) {
    emit('urlText', { id, url })
  }
}
</script>

<template>
  <draggable class="dragArea"
    tag="ul"
    :list="tasks"
    handle=".handle"
    :group="{ name: 'g1' }"
    item-key="name">
    <template #item="{ element }">
      <li class="drag-item">
        <p class="paragraph-item">
          <span class="handle">
            <img src="/dots.svg"
              alt="dots">
          </span>
          <span contenteditable
            :data-id="element.id"
            @blur="edit"
            @keydown.prevent.enter=""
            class="name-text">{{ element.name }}</span>
          <span contenteditable
            :data-id="element.id"
            @keydown.prevent.enter=""
            @blur="urlText"
            class="url-text">{{ element.url }}</span>
        </p>
        <nested-draggable :tasks="element.children"
          @edit="edit"
          @urlText="urlText" />
      </li>
    </template>
  </draggable>
</template>
<style scoped>
.dragArea {
  margin: 0;
  min-height: 0;
  list-style-type: none;
  padding: 10px;
  padding-top: 0;
  outline: 0.5px dashed rgba(0, 0, 0, 0.6);
}

.handle {
  height: 20px;
  display: block;
}

.handle img {
  height: auto;
  max-height: 20px;
  display: block;
  cursor: grab;
}

.dragArea .dragArea {
  min-height: 0;
  background: #fafafa
}

.dragArea :not(.sortable-chosen) .dragArea {
  min-height: 35px;
}

.drag-item {
  width: 100%;
  min-height: 35px;
}

.paragraph-item {
  margin: 0;
  display: flex;
  font-size: 14px;
  align-items: center;
  flex-wrap: wrap;
  gap: 10px;
  padding: 10px 0;
}

.button {
  border: none;
  background: #787878;
  color: #fff;
  font-weight: 100;
  cursor: pointer;
  font-size: 12px;
  padding: 3px 5px;
  border-radius: 6px;
  transition: all .2s ease-in-out;
}

.button:hover {
  background: #898989;
  transition: all .2s ease-in-out;
}

.url-text,
.name-text {
  position: relative;
  max-width: 100px;
  width: 100%;
  overflow: hidden;
  border-bottom: 1px solid #ccc;
}

.name-text {
  max-width: calc(100% - 140px);
}
</style>
