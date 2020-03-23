<template>
  <div>
    <h1>Uh-Oh!</h1>
    <h4>
      It Looks like you are experiencing some network issues, please click the
      back button and try again.
    </h4>
    <router-link :to="{ name: 'event-list' }"
      >Or go back to the home page</router-link
    >
  </div>
</template>

<script>
export default {}
</script>

<style></style>
