<template lang="">
  <!-- declarative rendering -->
  <div>{{ message }}</div>
  <hr />
  <!-- methods -->
  <div>{{ count }}</div>
  <button @click="count++">Add 1</button>
  <button @click="addCount">Add count</button>
  <hr />
  <!-- v-model -->
  <input v-model="message" />
  <!-- berikut adalah isi dari v-model yang 2 way data binding -->
  <input :value="message" @input="message = $event.target.value" />
  <hr />
  <!-- v-if dan v-else -->
  <div v-if="status">ini v-if tampil</div>
  <div v-else="status">ini v-else tidak tampil</div>
  <button @click="setStatus">tampilkan</button>
  <hr />
  <!-- v-for looping -->
  <div>
    <ol>
      <li v-for="(todo, index) in todos" :key="todo.id">
        {{ index }} - {{ todo.activity }}
      </li>
    </ol>
  </div>
  <hr />
  <!-- add component in vue -->
  <div>ini adalah parent component</div>
  <ChildComponent />
  <hr />
  <!-- props in vue component -->
  <ChildComponent text="ini adalah props" />
  <ChildComponent :text="message" />
  <hr />
  <!-- loop in vue component-->
  <ChildComponent v-for="todo in todos" :key="todo.id" :text="todo.activity" />
  <hr />
  <!-- life cycle hook in vue -->
  <div id="lifecycle">{{ msg }}</div>
  <button @click="msg = 'tulisan setelah di render'">update</button>
  <hr />
  <!-- v-once => agar data pada suatu element tidak bisa berubah atau vue akan merender data hanya sekali jadi sifatnya immutable-->
  <div v-once>{{ message }}</div>
  <button @click="message = 'test mengubah data'">update</button>
  <hr />
  <!-- v-html => merender html di dalam script vue -->
  <div v-html="msgHTML"></div>
  <hr />
  <!-- computed => untuk memproses sesuatu secara real time lalu ditampilkan -->
  <div>{{ total }}</div>
  <input v-model="num1" />
  <input v-model="num2" />
  <hr />
  <!-- computed with getter and setter -->
  <div>{{ discount }}</div>
  <input v-model="num1" />
  <input v-model="num2" />
  <button @click="addDiscount">add discount</button>
  <hr />
  <!-- whatcher => untuk melihat perubahan pada sebuah data -->
  <div>{{ message }}</div>
  <input v-model="message" />
  <hr />

  <!-- dynamic class -->
  <div :class="status ? 'success' : 'failed'">status</div>
  <button @click="status = !status">change</button>
  <div :class="[boldClass, greenClass]">Test Merge Class</div>
  <hr />

  <!-- menjalankan Refs event pada vue -->
  <!-- refs => untuk memanggil methods dari child component melalui parent compontent -->
  <div>ini adalah event dari child</div>
  <EventComponent ref="EventRefs" @child-click="onPress" />
  <button @click="clickRefs">Tombol refs</button>
</template>
<script>
import ChildComponent from "./components/ChildComponent.vue";
import EventComponent from "./components/RefsComponent.vue";

export default {
  components: { ChildComponent, EventComponent },
  data() {
    return {
      message: "hi world",
      count: 0,
      todos: [
        { id: 1, activity: "game" },
        { id: 2, activity: "belajar" },
        { id: 3, activity: "mandi" },
      ],
      msg: "text awal",
      msgHTML: "<span style='color: red'>text ini berwarna merah</span>",
      num1: 0,
      num2: 0,
      status: false,
      boldClass: "bold",
      greenClass: "success",
    };
  },
  methods: {
    addCount() {
      this.count++;
    },
    setStatus() {
      this.status = !this.status;
    },

    //computed get & set
    addDiscount() {
      this.discount = 50;
    },

    // Refs event component
    onPress(val) {
      console.log(val);
    },
    clickRefs() {
      this.$refs.EventRefs.onClickRefs();
    },
  },
  // lifecycle hook
  beforeCreate() {
    console.log("before create");
  },
  created() {
    console.log("created");
    console.log(this.msg);
    console.log(document.getElementById("lifecycle"));
  },
  beforeMount() {
    console.log("before mount");
    console.log(this.msg);
    console.log(document.getElementById("lifecycle"));
  },
  mounted() {
    console.log("mounted");
    console.log(this.msg);
    console.log(document.getElementById("lifecycle"));
  },
  beforeUpdate() {
    console.log("before update");
    console.log(document.getElementById("lifecycle").textContent);
  },
  updated() {
    console.log("updated");
    console.log(document.getElementById("lifecycle").textContent);
  },
  beforeUnmount() {
    console.log("before destroy");
  },
  unmounted() {
    console.log("destroyed");
  },

  //computed
  computed: {
    total() {
      // return parseInt(this.num1) + parseInt(this.num2)

      if (this.num1 > this.num2) {
        return "num1 lebih besar dari num2";
      } else if (this.num1 == this.num2) {
        return "num1 sama dengan num2";
      } else {
        return "num1 lebih kecil dari num2";
      }
    },
    discount: {
      get() {
        return parseInt(this.num1) + parseInt(this.num2);
      },
      set(value) {
        this.num1 -= value;
        this.num2 -= value;
      },
    },
  },

  // watcher
  watch: {
    message(value) {
      console.log("perubahan di message " + value);
    },
  },
};
</script>

<!-- dynamic class -->
<style>
.failed {
  color: red;
}
.success {
  color: green;
}
.bold {
  font-weight: 800;
}
</style>
