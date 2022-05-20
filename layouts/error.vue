<template>
  <div class="container-fluid">
    <!-- 404 Error Text -->
    <div class="text-center margs">
      <div class="error mx-auto" data-text="404">{{ error.statusCode }}</div>
      <p class="lead text-gray-800 mb-5">Page Not Found</p>
      <p class="text-gray-500 mb-0">It looks like you found a glitch in the matrix...</p>
      <nuxt-link to="/">&larr; Back to Dashboard</nuxt-link>
    </div>
  </div>
</template>
<script>
export default {
  props: ['error'],
}
</script>
<style>
.margs {
  margin-top: 10%;
}
</style>