<template>
<div class="admin">
  <h1>Add or update plants below</h1>
  <div class="addAndEdit">

    <div class="add">
      <div class="heading">
        <div class="circle">1</div>
        <h2>Add Your Plant</h2>
      </div>
      <div class="form">
        <input v-model="title" placeholder="Plant Name">

        <p></p>
        <input type="file" name="photo" @change="fileChanged">
        <p></p>
        <input v-model="species" placeholder="Species">
        <p></p>
        When was it last watered? <input type="date" v-model="lastWatered">
        <p></p>
        When was it last fertilized? <input type="date" v-model="lastFed">
        <p></p>
        <textarea v-model="description" placeholder="Notes"></textarea>
        <p></p>
        <button @click="upload">Upload</button>

      </div>
      <div class="upload" v-if="addItem">
        <p></p>
        <h3>Added!</h3>
        <p></p>
        <img :src="addItem.path" />
        <p></p>
        Name: {{addItem.title}}
        <p></p>
        Species: {{addItem.species}}
        <p></p>
        Last Watered: {{addItem.lastWatered}}
        <p></p>
        Last Fertilized: {{addItem.lastFed}}
        <p></p>
        Notes: {{addItem.description}}
      </div>
    </div>


    <div class="edit">
      <div class="heading">
        <div class="circle">2</div>
        <h2>Update Your Plant</h2>
      </div>
      <div class="form">
        <input v-model="findTitle" placeholder="Search">
        <div class="suggestions" v-if="suggestions.length > 0">
          <div class="suggestion" v-for="s in suggestions" :key="s.id" @click="selectItem(s)">{{s.title}}
          </div>
        </div>
      </div>
      <div class="upload" v-if="findItem">
        <p></p>
        <img :src="findItem.path" />
        <p></p>
        Name: <input v-model="findItem.title" placeholder="Name">
        <p></p>
        Species: <input v-model="findItem.species" placeholder="Species">
        <p></p>
        When was it last watered? <input type="date" v-model="findItem.lastWatered">
        <p></p>
        When was it last fertilized? <input type="date" v-model="findItem.lastFed">
        <p></p>
        <textarea v-model="findItem.description" placeholder="Notes"></textarea>
      </div>
      <div class="actions" v-if="findItem">
        <button @click="deleteItem(findItem)">Delete</button>
        <button @click="editItem(findItem)">Edit</button>
      </div>
    </div>
  </div>
</div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Admin',
  data() {
    return {
      title: "",
      file: null,
      description: "",
      lastFed: "",
      lastWatered: "",
      species: "",
      addItem: null,
      items: [],
      findTitle: "",
      findItem: null,
    }
  },
  created() {
    this.getItems();
  },
  computed: {
    suggestions() {
      let items = this.items.filter(item => item.title.toLowerCase().startsWith(this.findTitle.toLowerCase()));
      return items.sort((a, b) => a.title > b.title);
    }
  },
  methods: {
    async editItem(item) {
      try {
        await axios.put("/api/items/" + item._id, {
          title: this.findItem.title,
          description: this.findItem.description,
          species: this.findItem.species,
          lastFed: this.findItem.lastFed,
          lastWatered: this.findItem.lastWatered,
        });
        this.findItem = null;
        this.getItems();
        return true;
      } catch (error) {
        console.log(error);
      }
    },
    selectItem(item) {
      this.findTitle = "";
      this.findItem = item;
    },
    async deleteItem(item) {
      try {
        await axios.delete("/api/items/" + item._id);
        this.findItem = null;
        this.getItems();
        return true;
      } catch (error) {
        console.log(error);
      }
    },
    async getItems() {
      try {
        let response = await axios.get("/api/items");
        this.items = response.data;
        return true;
      } catch (error) {
        console.log(error);
      }
    },
    fileChanged(event) {
      this.file = event.target.files[0]
    },
    async upload() {
      try {
        const formData = new FormData();
        formData.append('photo', this.file, this.file.name)
        let r1 = await axios.post('/api/photos', formData);
        let r2 = await axios.post('/api/items', {
          title: this.title,
          description: this.description,
          species: this.species,
          lastFed: this.lastFed,
          lastWatered: this.lastWatered,
          path: r1.data.path
        });
        this.addItem = r2.data;
      } catch (error) {
        console.log(error);
      }
    },
  }
}
</script>


<style scoped>

/* Suggestions */
.suggestions {
  width: 200px;
  border: 1px solid #ccc;
}

.suggestion {
  min-height: 20px;
}

.suggestion:hover {
  background-color: #5BDEFF;
  color: #fff;
}

.image h2 {
  font-style: italic;
  font-size: 25px;
}

.image h3 {
  font-style: italic;
  font-size: 15px;
}

.heading {
  display: flex;
  margin-bottom: 20px;
  margin-top: 20px;
}

.heading h2 {
  margin-top: 8px;
  margin-left: 10px;
}

.addAndEdit {
  display: flex;
  justify-content: space-around;
}

.add,
.edit {
  flex-direction: column;
  display: flex;
}

.circle {
  border-radius: 50%;
  width: 18px;
  height: 18px;
  padding: 8px;
  background: #333;
  color: #fff;
  text-align: center
}

/* Form */
input,
textarea,
select,
button {
  font-family: 'Montserrat', sans-serif;
  font-size: 1em;
}


.form {
  margin-right: 50px;
}

/* Uploaded images */
.upload h2 {
  margin: 0px;
}

.upload img {
  max-width: 300px;
}
</style>
