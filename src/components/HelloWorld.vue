<template>
  <div class="hello">
    <Authorized :authority="['admin']">
      <h1>{{ msg }}</h1>
      123456
    </Authorized>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String
  }
};
</script>
