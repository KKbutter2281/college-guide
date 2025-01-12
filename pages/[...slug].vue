<template>
  <div class="flex min-h-screen bg-white dark:bg-zinc-950">
    <!-- Left sidebar (primary navigation) -->
    <aside class="fixed inset-y-0 z-50 flex w-72 flex-col border-r border-zinc-200 dark:border-zinc-800 bg-white dark:bg-zinc-950">
      <!-- Logo area -->
      <div class="border-b border-zinc-200 dark:border-zinc-800 px-6 py-4 flex items-center gap-2">
        <span class="font-semibold text-lg">Your Logo</span>
      </div>

      <!-- Search -->
      <div class="p-4 border-b border-zinc-200 dark:border-zinc-800">
        <div class="relative">
          <svg 
            class="absolute left-3 top-1/2 -translate-y-1/2 w-4 h-4 text-zinc-500"
            xmlns="http://www.w3.org/2000/svg" 
            fill="none" 
            viewBox="0 0 24 24" 
            stroke="currentColor"
          >
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
          </svg>
          <input
            type="search"
            placeholder="Quick search..."
            class="w-full pl-9 pr-4 py-2 text-sm bg-zinc-50 dark:bg-zinc-900 rounded-lg border border-zinc-200 dark:border-zinc-800 focus:outline-none focus:ring-2 focus:ring-blue-500 dark:focus:ring-blue-400 focus:border-transparent dark:placeholder-zinc-400"
            v-model="searchQuery"
            @input="handleSearch"
          />
        </div>
      </div>

      <!-- Navigation -->
      <nav class="flex-1 overflow-y-auto py-6 px-4">
        <ContentNavigation v-slot="{ navigation }">
          <div v-for="section in navigation" :key="section._path" class="mb-6">
            <!-- Section title -->
            <div class="mb-2 px-2 flex items-center">
              <h2 class="font-semibold text-sm text-zinc-900 dark:text-zinc-200">
                {{ section.title }}
              </h2>
            </div>

            <!-- Section links -->
            <div class="space-y-1">
              <NuxtLink
                :to="section._path"
                class="block rounded-md px-2 py-1.5 text-sm"
                :class="[
                  currentPath === section._path
                    ? 'text-blue-600 dark:text-blue-400 bg-blue-50 dark:bg-blue-500/10 font-medium'
                    : 'text-zinc-700 dark:text-zinc-400 hover:text-zinc-900 dark:hover:text-zinc-300 hover:bg-zinc-50 dark:hover:bg-zinc-900'
                ]"
              >
                Overview
              </NuxtLink>

              <template v-if="section.children">
                <NuxtLink
                  v-for="child in section.children"
                  :key="child._path"
                  :to="child._path"
                  class="block rounded-md px-2 py-1.5 text-sm"
                  :class="[
                    currentPath === child._path
                      ? 'text-blue-600 dark:text-blue-400 bg-blue-50 dark:bg-blue-500/10 font-medium'
                      : 'text-zinc-700 dark:text-zinc-400 hover:text-zinc-900 dark:hover:text-zinc-300 hover:bg-zinc-50 dark:hover:bg-zinc-900'
                  ]"
                >
                  {{ child.title }}
                </NuxtLink>
              </template>
            </div>
          </div>
        </ContentNavigation>
      </nav>
    </aside>

    <!-- Mobile nav overlay -->
    <div
      v-if="showMobileMenu"
      class="fixed inset-0 z-40 bg-zinc-950/50 backdrop-blur-sm"
      @click="toggleMobileMenu"
    ></div>

    <!-- Main content -->
    <div class="flex flex-1">
      <div class="relative flex-1 md:ml-72">
        <!-- Top bar -->
        <header class="sticky top-0 z-30 flex h-14 items-center gap-4 border-b border-zinc-200 dark:border-zinc-800 bg-white/95 dark:bg-zinc-950/95 backdrop-blur supports-backdrop-blur:bg-white/60 px-4 sm:px-6 lg:px-8">
          <!-- Mobile menu button -->
          <button
            type="button"
            class="md:hidden flex h-6 w-6 items-center justify-center rounded-md text-zinc-500"
            @click="toggleMobileMenu"
          >
            <span class="sr-only">Toggle navigation</span>
            <svg class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" d="M3.75 6.75h16.5M3.75 12h16.5M3.75 17.25h16.5" />
            </svg>
          </button>
        </header>

        <!-- Main content area -->
        <main class="relative">
          <!-- Article -->
          <div class="mx-auto max-w-3xl px-6 py-10">
            <article class="prose prose-zinc dark:prose-invert max-w-none">
              <ContentDoc />
            </article>
          </div>

          <!-- Right sidebar (on-page navigation) -->
          <div class="fixed top-14 bottom-0 right-0 hidden xl:block xl:w-72 overflow-y-auto border-l border-zinc-200 dark:border-zinc-800 bg-white dark:bg-zinc-950 py-8 px-6">
            <h3 class="text-sm font-semibold text-zinc-900 dark:text-zinc-100 mb-4">On this page</h3>
            <ContentToc />
          </div>
        </main>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import { useRoute } from 'vue-router'

const route = useRoute()
const searchQuery = ref('')
const showMobileMenu = ref(false)

const currentPath = computed(() => route.path)

const handleSearch = () => {
  // Implement search functionality
  console.log('Searching for:', searchQuery.value)
}

const toggleMobileMenu = () => {
  showMobileMenu.value = !showMobileMenu.value
}
</script>

<style>
/* Core prose styles */
.prose {
  max-width: none;
}

.prose p, .prose ul, .prose ol {
  text-wrap: pretty;
}

.prose h1 {
  @apply font-bold text-3xl sm:text-4xl mb-8 font-display tracking-tight;
}

.prose h2 {
  @apply text-2xl font-semibold mb-4 mt-8 font-display tracking-tight scroll-mt-32;
}

.prose h3 {
  @apply text-xl font-semibold mb-4 mt-6 tracking-tight scroll-mt-32;
}

.prose h4 {
  @apply text-lg font-semibold mb-4 mt-6 tracking-tight;
}

.prose a {
  @apply font-medium text-blue-600 dark:text-blue-400 no-underline hover:underline;
}

.prose strong {
  @apply font-semibold;
}

.prose pre {
  @apply rounded-lg border border-zinc-200 dark:border-zinc-800 bg-zinc-900 dark:bg-zinc-900/50;
}

.prose code {
  @apply rounded px-1.5 py-0.5 font-mono text-sm font-normal bg-zinc-100 dark:bg-zinc-800 text-zinc-900 dark:text-zinc-200;
}

.prose pre code {
  @apply bg-transparent dark:bg-transparent p-0 text-inherit;
}

.prose table {
  @apply w-full text-left border-collapse;
}

.prose th {
  @apply font-semibold p-3 border-b-2 border-zinc-200 dark:border-zinc-800 bg-zinc-50 dark:bg-zinc-900;
}

.prose td {
  @apply p-3 border-b border-zinc-200 dark:border-zinc-800;
}

.prose blockquote {
  @apply border-l-2 border-zinc-200 dark:border-zinc-700 pl-4 italic;
}

/* Table of contents styles */
.table-of-contents {
  @apply text-sm space-y-2 w-full;
}

.table-of-contents a {
  @apply block py-1 text-zinc-600 dark:text-zinc-400 hover:text-zinc-900 dark:hover:text-zinc-200 no-underline;
}

.table-of-contents a.active {
  @apply text-blue-600 dark:text-blue-400 font-medium;
}

.table-of-contents ol {
  @apply space-y-2 mt-2;
}

.table-of-contents li {
  @apply leading-5;
}

.table-of-contents li ol {
  @apply pl-4 mt-2;
}
</style>