<template>
  <NuxtLayout>
    <div class="prose">
      <h1>Dang</h1>
      <p>It looks like something broke.</p>
      <p>Sorry about that.</p>
      <p>
        <strong>{{ error?.message }}</strong>
      </p>
      <p>
        Go to the
        <a class="hover:cursor-pointer" @click="handleError"> First lesson</a>
      </p>
    </div>
  </NuxtLayout>
</template>

<script lang="ts" setup>
const error = useError();
const handleError = () => {
  clearError({
    redirect: useFirstLesson().path,
  });
};
</script>
