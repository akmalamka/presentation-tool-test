<script setup lang="ts">
const query = groq`*[_type == "post"]`
const { data } = useSanityQuery(query)
watchEffect(() => console.log(data.value))
</script>

<template>
  <div>
    <h1>{{data?.[0].title}}</h1>
  </div>
</template>