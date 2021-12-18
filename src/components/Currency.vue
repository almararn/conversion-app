<template>
  <div class="currency">{{ currency }}</div>
</template>

<script>
export default {
  props: ["currency"]
};
</script>

<style>
.currency {
  font-size: 14px;
  text-align: center;
  font-style: italic;
  color: gray;
}
</style>