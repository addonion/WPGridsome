<template>
  <div>
    <ul v-if="pageInfo" class="flex justify-center w-full gap-1 text-gray-800 pager">
      <li v-for="i in Math.ceil(pageInfo.total / perPage)" :key="i">
        <g-link class="inline-block p-2 leading-none text-indigo-700 rounded dark:text-gray-200" :class="{ active: pageInfo.currentPage == i }" :to="i === 1 ? `/posts` : `/posts/page/${i}`">{{ i }}</g-link>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: ["pageInfo", "perPage"],
};
</script>

<style lang="postcss">
.pager .active--exact,
.pager a:hover {
  @apply bg-indigo-500 text-white;
}
</style>
