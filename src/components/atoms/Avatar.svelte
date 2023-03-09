<script>
  export let src = "";
  export let size = "40px";
</script>

<img src={src} alt="Avatar" style="width: {size}; height: {size}">
<style>
  img {
    border-radius: 50%;
    object-fit: cover;
    box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.3);
  }
</style>