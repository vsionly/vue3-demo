<template>
  <div class="about">
    <h1>This is an about page</h1>
  </div>
</template>
<script>
// @ is an alias to /src

export default {
  name: "AboutView",
  created() {
    let o = {
      ew: { a: 1, b: 2 },
      w: { a: 3, b: 44 }
    };
    console.log(o.keys(), 333);
  }
};
</script>