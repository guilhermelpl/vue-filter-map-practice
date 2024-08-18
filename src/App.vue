<template>
  <div>
    <h2>Name List</h2>

    <Search @searching-user="searching" />

    <ul>
      <li v-for="user in users['data']" :key="user.id"
        :class="{ highlight: highlight.length > 0 && user.firstName.toLocaleLowerCase().includes(highlight.toLocaleLowerCase()) }">
        {{ user.firstName }} {{ user.lastName }}
      </li>
    </ul>
  </div>

</template>

<script setup>
import { reactive, onMounted, ref } from "vue"
import Search from './components/Search.vue'
import fakeData from './assets/data/MOCK_DATA.json'

const users = reactive({ data: [] })
const highlight = ref('');

onMounted(() => {
  users["data"] = fakeData;
})

function searching(searched) {
  if (!searched.searching.length) {
    highlight.value = "";
    users["data"] = fakeData;
    return;
  }
  if (searched.type === 'search') {
    users["data"] = users["data"].filter((item) =>
      item.firstName.toLocaleLowerCase().includes(searched.searching.toLocaleLowerCase())
    )
  } else {
    highlight.value = searched.searching
  }
}

</script>


<style>
ul {
  list-style: none;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

ul li {
  background-color: aliceblue;
  color: #000;
  padding: 0.3rem;
  margin-right: 0.1rem;
  margin-bottom: 0.3rem;
  border: solid 0.06rem #000;
  border-radius: 0.3rem;
}

.highlight {
  background-color: blueviolet;
  color: cornsilk
}
</style>
