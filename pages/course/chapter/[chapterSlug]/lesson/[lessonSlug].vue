<template>
  <div>
    <p class="mt-0 uppercase font-bold text-slate-400 mb-1">
      Lesson {{ chapter?.number }} - {{ lesson?.number }}
    </p>
    <h2 class="my-0">{{ lesson?.title }}</h2>
    <div class="flex space-x-4 mt-2 mb-8">
      <NuxtLink
        v-if="lesson?.sourceUrl"
        class="font-normal text-md text-gray-500"
        :href="lesson?.sourceUrl"
      >
        Download Source Code
      </NuxtLink>
      <NuxtLink
        v-if="lesson?.downloadUrl"
        class="font-normal text-md text-gray-500"
        :href="lesson?.downloadUrl"
      >
        Download Video
      </NuxtLink>
    </div>
    <VideoPlayer v-if="lesson?.videoId" :video-id="lesson.videoId" />
    <p>{{ lesson?.text }}</p>
    <LessonCompleteButton
      :model-value="isLessonComplete"
      @update:model-value="toggleComplete"
    />
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

const title = computed((): string => {
  return `${lesson.value?.title} - ${course.title}` || 'Unknown Page';
});

useHead({
  title,
});

const progress = useState('progress', () => {
  return [] as Array<boolean[]>;
});

const isLessonComplete = computed(() => {
  if (chapter.value && !progress.value[chapter.value.number - 1]) {
    return false;
  }

  if (
    chapter.value &&
    lesson.value &&
    !progress.value[chapter.value.number - 1][lesson.value.number - 1]
  ) {
    return false;
  }

  return (
    chapter.value &&
    lesson.value &&
    progress.value[chapter.value.number - 1][lesson.value.number - 1]
  );
});

const toggleComplete = () => {
  if (chapter.value && lesson.value) {
    if (!progress.value[chapter.value.number - 1]) {
      progress.value[chapter.value.number - 1] = [];
    }

    progress.value[chapter.value.number - 1][lesson.value.number - 1] =
      !isLessonComplete.value;
  }
};
</script>
