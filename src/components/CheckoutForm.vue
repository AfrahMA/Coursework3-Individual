<template>
  <div>
    <div v-for="(lesson, index) in cart" :key="index">
      <h2 v-text="lesson.title"></h2>
      <figure>
        <img v-bind:src="lesson.image" />
      </figure>
      <p v-html="lesson.location"></p>
      <p>Price: {{ lesson.price }}</p>
      <button v-on:click="removeFromTheCart(index)">Remove Lesson</button>
    </div>
    <div>
      <h2>Checkout</h2>
      <p>
        <strong>First Name:</strong>
        <input v-model.trim="order.firstName" />
      </p>
      <p>
        <strong>Last Name:</strong>
        <input v-model.trim="order.lastName" />
      </p>
      <p>
        <strong>Phone Number:</strong>
        <input v-model.number="order.phoneNumber" type="number" />
      </p>

      <h2>Order Information</h2>
      <p>First Name: {{ order.firstName }}</p>
      <p>Last Name: {{ order.lastName }}</p>
      <p>Phone Number: {{ order.phoneNumber }}</p>

      <button v-on:click="newOrderFunction">Place Order</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "CheckoutForm",
  props: ["cart"],
  data() {
    return {
      order: {
        firstName: "",
        lastName: "",
        phoneNumber: "",
      },
    };
  },
  methods: {
    removeFromTheCart(index) {
      this.$emit("removeLesson", index);
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
    newOrderFunction() {
      const newOrder = {
        firstName: this.order.firstName,
        lastName: this.order.lastName,
        phoneNumber: this.order.phoneNumber,
        lessons: this.cart.map((item) => {
          return { _id: item._id, lesson: item.lesson, spaces: item.spaces };
        }),
      };

      fetch(
        "https://coursework2afterschoolclub.herokuapp.com/collection/orders",
        {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          v: JSON.stringify(newOrder),
        }
      )
        .then((response) => response.json())
        .then((responseJSON) => {
          document.getElementById("response").innerText =
            JSON.stringify(responseJSON);
        })
        .catch((error) => {
          document.getElementById("error").innerText = error;
        });
    },
  },
};
</script>

<style>
</style>