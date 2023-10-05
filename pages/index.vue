<script setup>
import { ref } from 'vue'
// só pra mudar o título da página
useHead({
  title: 'Link Tree'
})

const links = ref([
  {
    id: 1,
    name: 'Link 1',
    url: '',
    status: false,
    children: [
    ]
  },
  {
    id: 2,
    name: 'Link 2',
    url: '',
    status: false,
    children: [
    ]
  }
])

const countAllLinks = (links) => {
  let count = 0
  const countLinks = ({ children }) => {
    count += 1
    if (children && children.length > 0) {
      children.forEach(countLinks)
    }
  }
  links.forEach(countLinks)
  return count
}

const addLink = () => {
  const flat = links.value.flat(Infinity)
  const count = countAllLinks(flat)
  links.value.push({
    id: count + 1,
    name: `Link ${count + 1}`,
    children: [],
    status: false,
    url: ''
  })
}

const findElementById = (links, id) => {
  for (const link of links) {
    if (link.id === id) {
      return link
    } else if (link.children && link.children.length > 0) {
      const result = findElementById(link.children, id)
      if (result) return result
    }
  }
  return null
}

const edit = ({ id, newName }) => {
  const linkToEdit = findElementById(links.value, id)
  if (linkToEdit) {
    linkToEdit.name = newName
  }
}

const urlText = ({ id, url }) => {
  const linkToEdit = findElementById(links.value, id)
  if (linkToEdit) {
    linkToEdit.url = url
  }
}

</script>
<template>
  <div class="box-area">
    <h1>Protótipo de Link Tree</h1>
    <div class="box-components">
      <LinkTree :list="links"
        @add-link="addLink"
        @edit="edit"
        @urlText="urlText" />
      <div class="link-tree-preview">
        <LinkTreePreview :links="links" />
      </div>
    </div>
  </div>
</template>

<style>
.link-tree-preview {
  width: calc(50% - 10px);
  background: #fafafa;
  border: 1px solid #ccc;
}

.link-tree-preview ul {
  padding-left: 10px;
  list-style-type: none;
}

.box-area {
  width: 100%;
  position: relative;
  padding: 20px;
}

.submit-button {
  margin-top: 10px;
  border: none;
  background: #787878;
  color: #fff;
  font-weight: 100;
  cursor: pointer;
  font-size: 14px;
  padding: 8px 12px;
  margin: 20px auto 0;
  border-radius: 6px;
  transition: all .2s ease-in-out;
}

.flex-button {
  display: flex;
  gap: 10px;
  align-items: center;
  flex-wrap: wrap;
  margin-top: 20px;
}

.flex-button label {
  font-size: 14px;
}

.flex-button input {
  font-size: 14px;
  padding: 5px 8px;
}

.box-area h1 {
  color: #404042;
}

.box-components {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  gap: 20px;
  align-items: flex-start;
}
</style>