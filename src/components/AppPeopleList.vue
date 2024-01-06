<template>
  <div v-if="people.length !== 0">
    <div class="card inline" v-for="{ id, firstName } in people" :key="id">
      <h3>
        {{ firstName }}
      </h3>
      <div class="btn danger" @click="$emit('remove', id)">DELL</div>
    </div>
  </div>
  <div class="card" v-else>
    <h4>It's empty here now.</h4>
    <button class="btn" @click="$emit('load')">Download the list</button>
  </div>
</template>

<script>
export default {
  emits: ["load", "remove"],
  props: ["people"],
};
</script>

<style scoped>
.inline {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
