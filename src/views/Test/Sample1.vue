<template>
  <div id="example-4">
    <input type="radio" id="one" value="One1" v-model="picked" />
    <label for="one">One</label>
    <br />
    <input type="radio" id="two" value="Two2" v-model="picked" />
    <label for="two">Two</label>
    <br />
    <select v-model="picked">
      <option value="黑桃">黑桃</option>
      <option value="红心">红心</option>
      <option value="梅花">梅花</option>
      <option value="方片">方片</option>
    </select>
    <br />
    <span>Picked: {{ picked }}</span>
  </div>
</template>

<script>
export default {
  data() {
    return {
      picked: "红心"
    };
  }
};
</script>

<style></style>
