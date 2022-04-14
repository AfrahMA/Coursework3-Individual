<template>
  <div>
    <div v-for="lesson in lessons" :key="lesson.id">
      <p v-text="lesson.lesson"></p>
      <figure>
        <img v-bind:src="lesson.image" style="width: 25%; height: 25%" />
      </figure>
      <p v-text="lesson.location"></p>
      <p>Price: {{ lesson.price }}</p>
      <p>Spaces: {{ lesson.spaces - cartCount(lesson.id) }}</p>

      <button v-on:click="addIntoCart(lesson)" v-if="canAddToCart(lesson)">
        Add to cart
      </button>

      <button disabled="disabled" v-else>Add to cart</button>

      <span v-if="lesson.spaces === cartCount(lesson.id)">All out!</span>
      <span v-else-if="lesson.spaces - cartCount(lesson.id) < 5">
        Only {{ lesson.spaces - cartCount(lesson.id) }} left!
      </span>
      <span v-else> Buy now!</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "LessonList",
  props: ["lessons"],
  data() {
    return {
      cart: [],
    };
  },
  methods: {
    addIntoCart(lesson) {
      this.$emit("addLesson", lesson);
    },
    canAddToCart(lesson) {
      return lesson.spaces > this.cartCount(lesson.id);
    },
    cartCount(lesson) {
      let count = 0;
      for (let i = 0; i < this.cart.length; i++) {
        if (this.cart[i] === lesson) {
          count++;
        }
      }
      return count;
    },
  },
};
</script>

<style></style>
