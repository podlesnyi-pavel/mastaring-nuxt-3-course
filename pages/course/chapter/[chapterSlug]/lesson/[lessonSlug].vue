<template>
  <div>
    <p class="mt-0 uppercase font-bold text-slate-400 mb-1">
      Lesson {{ chapter && chapter.number }} - {{ lesson && lesson.number }}
    </p>
    <h2 class="my-0">{{ lesson && lesson.title }}</h2>
    <div class="flex space-x-4 mt-2 mb-8">
      <a
        v-if="lesson && lesson.sourceUrl"
        class="font-normal text-md text-gray-500"
        :href="lesson && lesson.sourceUrl"
      >
        Download Source Code
      </a>
      <a
        v-if="lesson && lesson.downloadUrl"
        class="font-normal text-md text-gray-500"
        :href="lesson && lesson.downloadUrl"
      >
        Download Video
      </a>
    </div>
    <p>{{ lesson && lesson.text }}</p>
  </div>
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
  return chapter.value?.lessons.find(
    (lesson) => lesson.slug === route.params.lessonSlug
  );
});
</script>
