<script setup>
defineProps({
  msg: {
    type: String,
    required: true
  }
})
</script>

<template>
  <div class="greetings">
    <a class="green" href="https://findneteasecloudmusicuser.nclgclub.com/" target="_blank">
      <h1>{{ msg }}</h1>
    </a>
    <h3>
      很多时候，令我们感到痛苦的不是<span> TA </span>不爱你，
      而是那个爱着<span> TA </span>的自己却只能小心地去偷偷关注<span> TA </span>的各种动态。
    </h3>
  </div>
</template>

<style scoped>
span {
  color: hsla(160, 100%, 37%, 1)
}

h1 {
  font-weight: 500;
  font-size: 2.6rem;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {

  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
