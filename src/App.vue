<template>
  <div>
    <header>
      <h1 v-text="sitename"></h1>
      <button v-on:click="showCheckout">
        {{ cartItemCount }}
        <span class="fas fa-cart-plus"></span> Shopping Cart
      </button>
    </header>
    <p>hello</p>
    <LessonList
      v-if="showSubject"
      v-bind:lessons="lessons"
      @addLesson="addLesson"
    />
    <CheckoutForm v-else
     v-bind:cart="cart"
    @removeLesson="removeFromCart" />
  </div>
</template>

<script>
import LessonList from "./components/LessonList.vue";
import CheckoutForm from "./components/CheckoutForm.vue";

export default {
  name: "App",
  components: {
    LessonList,
    CheckoutForm,
  },
  data() {
    return {
      lessons: [],
      cart: [],
      showSubject: true,
    };
  },
  created() {
    fetch("http://localhost:3000/collection/lessons")
      .then((res) => res.json())
      .then((data) => (this.lessons = data));
  },
  methods: {
    addLesson(lesson) {
      this.cart.push(lesson);
      console.log("addLesson to shopping cart");
      lesson.spaces--;
    },
    removeFromCart(index, lesson) {
      this.cart.splice(index, 1);
      lesson.spaces++;
      console.log(index, "removeLesson from shopping cart");
    },
    showCheckout() {
      this.showSubject = this.showSubject ? false : true;
    },
  },
  computed: {
    cartItemCount() {
      return this.cart.length;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
