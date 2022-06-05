<template>
  <div id="app">
    <div class="container">
      <h3>Shopping List</h3>
      <hr />
      <form action="">
        <input v-model="itemName" type="text" placeholder="Shop item">
        <button @click="addItem">Add Item</button>
      </form>
      <ul>
        <li @dblclick="deleteItem(item.id)" v-for="item in items" :key="item.id">
          {{ item.name }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      items: [],
      itemName: '',
      url: 'http://localhost:3000/items'

    };
  },
  async created() {
    try {
      const res = await axios.get(this.url);
      this.items = res.data;
    } catch (err) {
      console.log(err);
    }
  },
  methods: {
    async addItem() {
      try {
        const res = await axios.post(this.url, {name: this.itemName})
        this.items = [...this.items, res.data]
        this.itemName = ''
      }
      catch(err) {
        console.log(err)
      }
      
    },

    deleteItem(id) {
      axios.delete(`${this.url}/${id}`)
      this.items = this.items.filter(item => item.id !== id)
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}

.container {
  width: 200px;
  margin: 0 auto;
}

ul {
  text-align: left;
}

button {
  background : #d5d3ec;
  border: none;
  padding: 10px 15px;
  margin-top: 5px;
}


</style>
