<template> 
  <div>
    <wholeApp />
  </div>
</template>

<script>
  import wholeApp from '@/components/wholeApp.vue';
  export default {
    name: "App",
    components: {
      wholeApp
    },
  };
</script>

<style>
body{
  margin: 0;
  padding: 0;
  background: #f1f1f1;
  color:#fff;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}
</style>
