<template>
  <div class="element-wrapper">
    <div class="removeFloor-wrapper">
      <button @click="removeFloor(item.id)" class="removeFloor">
        <img :src="remove" alt="remove" />
      </button>
    </div>
    <div class="header">Piętro: {{ item.floor }}</div>
    <div class="container">
      <Rooms
        v-for="el in rooms"
        :key="el.id"
        :el="el"
        :edit="edit"
        :remove="remove"
        @removeClicked="removeItem"
        class="room-container"
      />
    </div>
    <div v-show="this.edit" class="form-wrapper">
      <input
        type="text"
        class="input"
        placeholder="Dodaj dodatkowe pokoje"
        v-model="tags"
      />
      <button class="addButton" @click="addTags">Dodaj</button>
    </div>
    <div v-show="this.edit">
      Wpisz dodatkowe pokoje, rozdzielając nazwy przecinkiem
    </div>
    <div class="error">{{ error }}</div>
  </div>
</template>
<script>
import Rooms from "./Rooms.vue";
import remove from "../assets/remove.png";
export default {
  props: ["item", "edit"],
  name: "FloorElement",
  components: {
    Rooms
  },
  data() {
    return {
      firstRoom: this.item.firstRoom,
      lastRoom: this.item.lastRoom,
      tags: null,
      rooms: [],
      id: 0,
      error: null,
      existing: [],
      remove: remove,
      test: []
    };
  },
  methods: {
    addRooms: function() {
      for (let i = this.firstRoom; i <= this.lastRoom; i++) {
        this.rooms.push({
          id: this.id,
          r: i
        });
        this.id++;
      }
    },
    addTags: function() {
      if (!this.tags) {
        this.error = "Pole nie może być puste!";
      } else {
        this.error = null;
        let tagsArr = this.tags.split(",");
        tagsArr = tagsArr.map(e => e.trim());
        tagsArr = new Set(tagsArr);
        tagsArr = [...tagsArr];
        let roomsArr = [];
        roomsArr = this.rooms.map(e => e.r.toString());
        tagsArr = tagsArr.filter(tag => !roomsArr.includes(tag));
        if (!tagsArr.length) {
          this.error = "Takie pokoje już istnieją!";
        } else {
          for (const element of tagsArr) {
            this.rooms.push({
              id: this.id,
              r: element
            });
            this.id++;
          }
        }
        this.tags = null;
      }
    },
    removeItem: function(id) {
      const roomId = this.rooms.findIndex(room => room.id === id);
      this.rooms.splice(roomId, 1);
    },
    removeFloor: function() {
      this.$emit("floorRemoved", this.item.id);
    }
  },
  beforeMount() {
    this.addRooms();
  }
};
</script>
<style scoped>
.element-wrapper {
  width: 80%;
  margin: 20px;
  padding: 20px;
  text-align: center;
  border: 3px solid #7bad9a;
}

.header {
  margin-bottom: 10px;
  font-size: 1.5rem;
  font-weight: 900;
  text-transform: uppercase;
}

.container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.form-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 20px;
}

.input {
  width: 60%;
  height: 30px;
  font-size: 1rem;
  border: none;
  border-bottom: 2px solid #7bad9a;
  background-color: transparent;
  outline: transparent;
}

.addButton {
  margin: 0 10px;
  border: none;
  border-radius: 20px;
  background-color: #7bad9a;
  width: 15%;
  padding: 10px;
  color: #fff;
  cursor: pointer;
  outline: #fff;
}

.error {
  color: red;
  text-align: center;
  font-size: 1.125rem;
}

.error-tag {
  display: inline-block;
  font-size: 1.125rem;
  margin: 10px;
  padding: 5px 15px;
  border: 1px solid black;
}

.removeFloor-wrapper {
  display: flex;
  justify-content: flex-end;
  width: 100%;
}

.removeFloor {
  border: none;
  background-color: #fff;
  cursor: pointer;
  outline: none;
}

.removeFloor img {
  background-color: #fff;
  width: 25px;
  width: 25px;
}
</style>
