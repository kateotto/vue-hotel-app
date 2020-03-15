<template>
  <div class="room-wrapper">
    <span class="room-element">{{ el.r }}</span>
    <br />
    <button v-show="this.edit" @click="removeItem(el.id)" class="remove-button">
      <img :src="remove" alt="remove" />
    </button>
  </div>
</template>
<script>
export default {
  props: ["el", "edit", "remove"],
  name: "Rooms",
  methods: {
    removeItem: function() {
      this.$emit("removeClicked", this.el.id);
    }
  }
};
</script>
<style scoped>
.room-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 10px;
  padding: 0 5px;
  min-width: 50px;
  max-width: 470px;
  height: 35px;
  border: 1px solid #7bad9a;
  cursor: pointer;
  overflow: hidden;
}

.room-element {
  padding: 0 10px;
  position: relative;
  font-size: 1.125rem;
  max-width: 450px;
}

.remove-button {
  display: flex;
  justify-content: center;
  height: 20px;
  width: 20px;
  border: none;
  cursor: pointer;
  background-color: transparent;
  outline: none;
  z-index: 1;
}

.remove-button img {
  width: 15px;
  height: 15px;
}
</style>
