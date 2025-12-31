<template>
 <div class="blog-post">
  <article class="post">
   <transition name="fade" mode="out-in">
    <header class="post-header">
     <div class="post-header-image">
      <img :src="post.image" :alt="post.title" />
      <div class="post-header-overlay">
       <div class="container">
        <div class="post-header-content">
         <div class="post-category">{{ post.category }}</div>
         <h1 class="post-title">{{ post.title }}</h1>
         <p class="post-description">{{ post.description }}</p>
         <div class="post-meta">
          <span class="post-author">By {{ post.author }}</span>
          <span class="post-date">{{ formatDate(post.date) }}</span>
         </div>
        </div>
       </div>
      </div>
     </div>
    </header>
   </transition>
   <div class="container">
    <div class="post-content">
     <!-- <div class="prose" v-html="post.content" /> -->
     {{ post.content }}
    </div>

    <div class="post-navigation">
     <NuxtLink to="/" class="back-button">
      <svg
       width="20"
       height="20"
       viewBox="0 0 24 24"
       fill="none"
       stroke="currentColor"
       stroke-width="2"
      >
       <line x1="19" y1="12" x2="5" y2="12"></line>
       <polyline points="12 19 5 12 12 5"></polyline>
      </svg>
      Back to All Posts
     </NuxtLink>
    </div>
   </div>
  </article>
 </div>
</template>

<script setup>
import { computed } from "vue";
import { blogPosts } from "~/data/blogPosts";
const route = useRoute();
const postsSorted = [...blogPosts].sort(
 (a, b) => new Date(b.date).getTime() - new Date(a.date).getTime()
);

const post = computed(() => postsSorted.find((p) => p._path === route.path));

if (!post.value) {
 throw createError({ statusCode: 404, statusMessage: "Post not found" });
}

const formatDate = (dateString) => {
 const options = { year: "numeric", month: "long", day: "numeric" };
 return new Date(dateString).toLocaleDateString("en-US", options);
};

useHead({
 title: post.value.title,
 meta: [{ name: "description", content: post.value.description }],
});
</script>

<style lang="scss" scoped>
@use "sass:color";
.blog-post {
 min-height: 100vh;
}

.post-header {
 .post-header-image {
  position: relative;
  height: 500px;
  overflow: hidden;

  @media (max-width: $breakpoint-mobile) {
   height: 400px;
  }

  img {
   width: 100%;
   height: 100%;
   object-fit: cover;
  }

  .post-header-overlay {
   position: absolute;
   top: 0;
   left: 0;
   right: 0;
   bottom: 0;
   background: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.3) 0%,
    rgba(0, 0, 0, 0.7) 100%
   );
   display: flex;
   align-items: flex-end;
   padding: $spacing-xl 0;

   @media (max-width: $breakpoint-mobile) {
    padding: $spacing-lg 0;
   }

   .post-header-content {
    color: $text-light;

    .post-category {
     display: inline-block;
     background-color: $primary-color;
     color: $text-light;
     padding: $spacing-xs $spacing-sm;
     border-radius: $border-radius;
     font-size: 0.875rem;
     font-weight: 600;
     margin-bottom: $spacing-md;
    }

    .post-title {
     font-size: 3rem;
     margin-bottom: $spacing-md;
     color: $text-light;

     @media (max-width: $breakpoint-mobile) {
      font-size: 2rem;
     }
    }

    .post-description {
     font-size: 1.25rem;
     margin-bottom: $spacing-md;
     opacity: 0.95;

     @media (max-width: $breakpoint-mobile) {
      font-size: 1rem;
     }
    }

    .post-meta {
     display: flex;
     gap: $spacing-md;
     font-size: 0.95rem;
     opacity: 0.9;

     @media (max-width: $breakpoint-mobile) {
      flex-direction: column;
      gap: $spacing-xs;
     }

     .post-author {
      font-weight: 600;
     }
    }
   }
  }
 }
}

.post-content {
 max-width: 800px;
 margin: 0 auto;
 padding: $spacing-xxl 0;

 @media (max-width: $breakpoint-mobile) {
  padding: $spacing-xl 0;
 }
}

.post-navigation {
 max-width: 800px;
 margin: 0 auto;
 padding-bottom: $spacing-xxl;

 @media (max-width: $breakpoint-mobile) {
  padding-bottom: $spacing-xl;
 }

 .back-button {
  display: inline-flex;
  align-items: center;
  gap: $spacing-xs;
  padding: $spacing-sm $spacing-md;
  background-color: $secondary-color;
  color: $text-dark;
  border-radius: $border-radius;
  font-weight: 500;
  transition: $transition;

  &:hover {
   background-color: color.adjust($secondary-color, $lightness: -5%);
   transform: translateX(-4px);
  }

  svg {
   transition: $transition;
  }
 }
}
.post-content {
 h1,
 h2,
 h3,
 h4,
 h5,
 h6 {
  color: $text-dark;
  margin-top: $spacing-lg;
  margin-bottom: $spacing-md;
 }

 h1 {
  font-size: 2.5rem;
  border-bottom: 2px solid $primary-color;
  padding-bottom: $spacing-sm;

  @media (max-width: $breakpoint-mobile) {
   font-size: 2rem;
  }
 }

 h2 {
  font-size: 2rem;
  color: $primary-color;

  @media (max-width: $breakpoint-mobile) {
   font-size: 1.75rem;
  }
 }

 h3 {
  font-size: 1.5rem;

  @media (max-width: $breakpoint-mobile) {
   font-size: 1.35rem;
  }
 }

 p {
  margin-bottom: $spacing-md;
  line-height: 1.8;
  color: #444;
 }

 ul,
 ol {
  margin-bottom: $spacing-md;
  padding-left: $spacing-lg;

  li {
   margin-bottom: $spacing-xs;
   line-height: 1.8;
   color: #444;
  }
 }

 ul {
  list-style-type: disc;
 }

 ol {
  list-style-type: decimal;
 }

 strong {
  font-weight: 700;
  color: $text-dark;
 }

 em {
  font-style: italic;
 }

 a {
  color: $primary-color;
  text-decoration: underline;

  &:hover {
   color: color.adjust($primary-color, $lightness: -10%);
  }
 }

 code {
  background-color: $secondary-color;
  padding: 2px 6px;
  border-radius: 4px;
  font-family: "Courier New", monospace;
  font-size: 0.9em;
 }

 pre {
  background-color: $secondary-color;
  padding: $spacing-md;
  border-radius: $border-radius;
  overflow-x: auto;
  margin-bottom: $spacing-md;

  code {
   background-color: transparent;
   padding: 0;
  }
 }

 blockquote {
  border-left: 4px solid $primary-color;
  padding-left: $spacing-md;
  margin: $spacing-md 0;
  font-style: italic;
  color: #666;
 }
}
.fade-enter-active,
.fade-leave-active {
 transition: opacity 1000ms ease;
}
.fade-enter-from,
.fade-leave-to {
 opacity: 0;
}
.fade-enter-to,
.fade-leave-from {
 opacity: 1;
}
</style>
