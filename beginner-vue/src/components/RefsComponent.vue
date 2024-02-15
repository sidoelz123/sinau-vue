<template>
  <div>ini adalah child</div>
  <button @click="onPress">Tombol Event</button>
</template>
<script>
export default {
  emits: ["child-click"],
  methods: {
    onPress() {
      this.$emit("child-click", "part ini adalah value");
    },
    onClickRefs() {
      console.log("click saya");
    },
  },
};
</script>
