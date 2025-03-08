<template>
  <q-layout>
    <Header></Header>
    <q-page-container class="fit" style="background: linear-gradient(to bottom, #35bad8 18%, white 0%); ">
      <Content></Content>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup>
import Header from 'components/AppHeader.vue'
import Content from 'components/AppContent.vue'

</script>

