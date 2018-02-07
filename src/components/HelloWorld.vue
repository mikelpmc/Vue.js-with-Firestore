<template>
  <div class="hello">
    <form @submit="addLocation(name, image)">
      <input type="text" v-model="name" placeholder="Location Name">
      <input type="text" v-model="image" placeholder="Location Image URL">
      <button type="submit">Add New location</button>
    </form>

    <br>

    <article v-for="(location, idx) in locations" :key="idx">
      <img :src="location.image">
      <h1>{{ location.name }}</h1>
      <button @click="deleteLocation(location.id)">Delete</button>
    </article>
  </div>
</template>

<script>
import { db } from '../main';

export default {
    name: 'HelloWorld',
    data() {
        return {
            locations: [],
            name: '',
            image: ''
        };
    },
    firestore() {
        return {
            locations: db.collection('locations').orderBy('createdAt')
        };
    },
    methods: {
        addLocation(name, image) {
            const createdAt = new Date();
            db.collection('locations').add({ name, image, createdAt });
        },
        deleteLocation(id) {
            db
                .collection('locations')
                .doc(id)
                .delete();
        }
    }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
    font-weight: normal;
}
ul {
    list-style-type: none;
    padding: 0;
}
li {
    display: inline-block;
    margin: 0 10px;
}
a {
    color: #42b983;
}

img {
    width: 250px;
}
</style>
