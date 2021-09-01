<template>
  <div
    class="item-products"
  >
    <img :src="food.image" class="img-fluid" />
    <div class="text-center">
      <strong>{{ food.name }}</strong> <br />
      <span>{{ food.description }}</span>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    food: {
      type: Object,
      default: () => {
        return {}
      },
    },
  },
}
</script>
