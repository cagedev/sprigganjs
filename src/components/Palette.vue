<template>
  <div class="container">
    <ul>
      <!-- iterate over wells -->
      <li v-for="(c, i) in colors" v-bind:key="c.id">{{ i }} - {{ c }}</li>
    </ul>
  </div>
</template>
