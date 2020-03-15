<template>
  <div class="container">
    <h2 class="title">
      Wpisz numery pięter i zakres numeracji pokojów/apartamentów, które na nich
      występują
    </h2>
    <div class="tooltip">
      <div class="tooltip-logo">
        <img :src="bulb" alt="Bulb-icon" />
      </div>
      <div class="tooltip-text">
        Aby edytować zakres lub wpisać poszczególne numery ręcznie, kliknij
        ikonę ołówka
      </div>
    </div>
    <div class="error">{{ error }}</div>
    <div class="input-container">
      <div class="floor-wrapper">
        <label for="floor">
          Piętro
          <input
            name="floor"
            type="number"
            class="floor"
            min="0"
            v-model="floorNr"
            placeholder="nr"
          />
        </label>
      </div>
      <div class="room-wrapper">
        <span>Numery pok./ap </span>
        <label for="startNr">
          od
          <input
            type="number"
            name="startNr"
            v-model="startNr"
            placeholder="nr"
          />
        </label>
        <label for="endNr">
          do
          <input type="number" name="endNr" v-model="endNr" placeholder="nr" />
        </label>
        <button class="pen-button" @click="changeEdit">
          <img :src="pencil" alt="save" />
        </button>
      </div>
    </div>
    <button @click="validateAndAddFloor" class="add-button">
      Dodaj piętro
    </button>
    <Floor
      v-for="item in rooms"
      :key="item.id"
      :item="item"
      :edit="edit"
      @floorRemoved="removeFloor"
    />
  </div>
</template>
<script>
import Floor from "./Floor.vue";
import bulb from "../assets/bulb.png";
import pencil from "../assets/pencil.png";
export default {
  name: "AppContainer",
  components: {
    Floor
  },
  data() {
    return {
      floorNr: null,
      startNr: null,
      endNr: null,
      rooms: [],
      error: "",
      edit: false,
      bulb: bulb,
      pencil: pencil
    };
  },
  methods: {
    validateAndAddFloor: function() {
      this.isFormValid() && this.addFloor() && this.clearFloor();
    },
    addFloor: function() {
      this.rooms.push({
        id: this.rooms.length,
        floor: this.floorNr,
        firstRoom: this.startNr,
        lastRoom: this.endNr
      });
      return true;
    },
    clearFloor: function() {
      this.floorNr = null;
      this.startNr = null;
      this.endNr = null;
      this.edit = false;
    },
    isFormValid: function() {
      this.error = null;
      if (!this.floorNr || !this.startNr || !this.endNr) {
        this.error = "Wypełnij wszystkie pola!";
        return false;
      }

      if (this.floorNr < 0 || this.floorNr === "-0") {
        this.error = "Piętro musi być dodatnie!";
        this.floorNr = null;
        return false;
      }

      if (this.startNr < 0 || this.startNr > this.endNr) {
        this.error = "Numery pokoi wypełnione błędnie!";
        this.startNr = null;
        this.endNr = null;
        return false;
      }

      if (this.rooms.findIndex(room => room.floor === this.floorNr) > -1) {
        this.error = "Takie piętro już istnieje!";
        this.floorNr = null;
        return false;
      }

      return true;
    },
    changeEdit: function() {
      this.edit = !this.edit;
    },
    removeFloor: function(id) {
      const floorId = this.rooms.findIndex(room => room.id === id);
      this.rooms.splice(floorId, 1);
    }
  }
};
</script>
<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 80%;
  height: auto;
  padding: 30px 0;
  margin: 30px 0;
  background-color: #fff;
  border-radius: 20px;
  box-shadow: 5px 5px 60px #555;
}

.title {
  width: 50%;
  text-align: center;
}

.tooltip {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  margin: 0px 30px;
}

.tooltip-logo img {
  width: 40px;
  height: 40px;
}

.tooltip-text {
  display: flex;
  align-content: center;
  text-align: center;
}

.input-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin: 25px 0;
}

.input-container input::-webkit-outer-spin-button,
::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

.input-container input[type="number"] {
  -moz-appearance: textfield;
}

.floor-wrapper {
  margin-bottom: 20px;
}

label {
  padding: 0 5px;
}

.floor-wrapper input,
.room-wrapper input {
  text-align: center;
  width: 40px;
  height: 30px;
}
.room-wrapper {
  display: flex;
  align-items: center;
}

.pen-button {
  border: none;
  background-color: transparent;
  cursor: pointer;
  outline-color: #7bad9a;
}

.pen-button img {
  width: 35px;
  height: 35px;
  margin: 0 20px;
}

.add-button {
  border: none;
  background-color: transparent;
  font-size: 1.25rem;
  font-weight: bold;
  cursor: pointer;
  margin: 5px 0;
  outline: transparent;
}

.add-button::after {
  content: "+";
  padding: 10px 15px;
  background-color: #7bad9a;
  color: #fff;
  border-radius: 20px;
  cursor: pointer;
}

.add-button:focus{
  box-shadow: 0 0 0 2pt #398569;
}

.error {
  color: red;
  font-size: 1.125rem;
  font-weight: bold;
}
</style>
