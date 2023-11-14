<template>
  <h2>Lesson</h2>
  <p>This is a lesson</p>
  <p>{{ chapter && chapter.title }}</p>
  <p>{{ lesson && lesson.title }}</p>
</template>

<script lang="ts" setup>
const course = useCourse();
const route = useRoute();

const chapter = computed(() => {
  return course.chapters.find(
    (chapter) => chapter.slug === route.params.chapterSlug
  );
});

const lesson = computed(() => {
  return (
    chapter.value &&
    chapter.value.lessons.find(
      (lesson) => lesson.slug === route.params.lessonSlug
    )
  );
});
</script>
