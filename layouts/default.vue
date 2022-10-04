<template>
  <v-app dark>
    <v-main>
      <Nuxt />
    </v-main>
    <client-only>
      <snackbar />
    </client-only>
  </v-app>
</template>

<script>
export default {
}
</script>
