
<script setup >
const user = useSupabaseUser()

watch(user, () => {
  if (user.value) {
      // Redirect to protected page
      return navigateTo('/')
  }
}, { immediate: true })
</script>

<template>
  <p>user {{ user }}</p>
  <div>Waiting for login...</div>
</template>