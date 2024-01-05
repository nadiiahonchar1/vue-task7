<template>
  <div v-if="people.length !== 0">
    <div class="card" v-for="{ id, firstName } in people" :key="id">
      <h3>
        {{ firstName }}
      </h3>
    </div>
  </div>
  <div class="card" v-else>
    <h4>It's empty here now.</h4>
    <button class="btn" @click="$emit('load')">Download the list</button>
  </div>
</template>

<script>
export default {
  emits: ["load"],
  props: ["people"],
};
</script>

<style lang="scss" scoped></style>
