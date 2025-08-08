<script setup>
  definePageMeta({
    middleware: [
      function () {
        return navigateTo('/dashboard');
      }
    ]
  })
</script>