<template>
 <div class="home">
  <section class="hero">
   <div class="container">
    <div class="hero-content">
     <h1 class="hero-title">Hill Walking Made Easy</h1>
     <p class="hero-description">
      Discover expert guides, essential tips, and inspiring routes to make your
      hill walking adventures safe, enjoyable, and unforgettable.
     </p>
    </div>
   </div>
  </section>

  <section class="posts-section">
   <div class="container">
    <div class="filter-section">
     <h2>Explore Our Guides</h2>
     <div class="filters">
      <button
       :class="['filter-btn', { active: selectedCategory === 'All' }]"
       @click="filterPosts('All')"
      >
       All Posts
      </button>
      <button
       v-for="category in categories"
       :key="category"
       :class="['filter-btn', { active: selectedCategory === category }]"
       @click="filterPosts(category)"
      >
       {{ category }}
      </button>
     </div>
    </div>

    <div class="posts-grid">
     <article
      v-for="post in displayedPosts"
      :key="post._path"
      class="post-card"
      @click="navigateToPost(post._path)"
     >
      <div class="post-image">
       <img :src="post.image" :alt="post.title" loading="lazy" />
       <div class="post-category">{{ post.category }}</div>
      </div>
      <div class="post-content">
       <h3>{{ post.title }}</h3>
       <p class="post-description">{{ post.description }}</p>
       <div class="post-meta">
        <span class="post-author">{{ post.author }}</span>
        <span class="post-date">{{ formatDate(post.date) }}</span>
       </div>
      </div>
     </article>
    </div>

    <div v-if="hasMorePosts" class="load-more-section">
     <button class="load-more-btn" @click="loadMore">Load More Posts</button>
    </div>
   </div>
  </section>
 </div>
</template>

<script setup>
import { ref, computed } from "vue";
import { blogPosts } from "~/data/blogPosts";

const posts = ref(
 blogPosts.sort(
  (a, b) => new Date(b.date).getTime() - new Date(a.date).getTime()
 )
);

console.log("Posts loaded:", posts.value.length);

const selectedCategory = ref("All");
const postsPerPage = 6;
const currentPage = ref(1);

const categories = computed(() => {
 const cats = new Set();
 posts.value?.forEach((post) => {
  if (post.category) {
   cats.add(post.category);
  }
 });
 return Array.from(cats).sort();
});

const filteredPosts = computed(() => {
 if (selectedCategory.value === "All") {
  return posts.value || [];
 }
 return (
  posts.value?.filter((post) => post.category === selectedCategory.value) || []
 );
});

const displayedPosts = computed(() => {
 return filteredPosts.value.slice(0, currentPage.value * postsPerPage);
});

const hasMorePosts = computed(() => {
 return displayedPosts.value.length < filteredPosts.value.length;
});

const filterPosts = (category) => {
 selectedCategory.value = category;
 currentPage.value = 1;
};

const loadMore = () => {
 currentPage.value++;
};

const formatDate = (dateString) => {
 const options = { year: "numeric", month: "long", day: "numeric" };
 return new Date(dateString).toLocaleDateString("en-US", options);
};

const navigateToPost = (path) => {
 navigateTo(path);
};
</script>

<style lang="scss" scoped>
@use "sass:color";
.hero {
 background: linear-gradient(
  135deg,
  $primary-color 0%,
  color.adjust($primary-color, $lightness: -10%) 100%
 );
 color: $text-light;
 padding: $spacing-xxl 0;
 text-align: center;

 @media (max-width: $breakpoint-mobile) {
  padding: $spacing-xl 0;
 }

 .hero-content {
  max-width: 800px;
  margin: 0 auto;
 }

 .hero-title {
  font-size: 3rem;
  margin-bottom: $spacing-md;
  animation: fadeInUp 0.8s ease;

  @media (max-width: $breakpoint-mobile) {
   font-size: 2rem;
  }
 }

 .hero-description {
  font-size: 1.25rem;
  opacity: 0.95;
  line-height: 1.8;
  animation: fadeInUp 0.8s ease 0.2s both;

  @media (max-width: $breakpoint-mobile) {
   font-size: 1rem;
  }
 }
}

@keyframes fadeInUp {
 from {
  opacity: 0;
  transform: translateY(30px);
 }
 to {
  opacity: 1;
  transform: translateY(0);
 }
}

.posts-section {
 padding: $spacing-xxl 0;

 @media (max-width: $breakpoint-mobile) {
  padding: $spacing-xl 0;
 }
}

.filter-section {
 margin-bottom: $spacing-xl;
 text-align: center;

 h2 {
  color: $text-dark;
  margin-bottom: $spacing-lg;
 }

 .filters {
  display: flex;
  flex-wrap: wrap;
  gap: $spacing-sm;
  justify-content: center;
 }

 .filter-btn {
  padding: $spacing-sm $spacing-md;
  background-color: $secondary-color;
  color: $text-dark;
  border: 2px solid transparent;
  border-radius: $border-radius;
  font-size: 1rem;
  font-weight: 500;
  cursor: pointer;
  transition: $transition;

  &:hover {
   background-color: color.adjust($secondary-color, $lightness: -5%);
   transform: translateY(-2px);
  }

  &.active {
   background-color: $primary-color;
   color: $text-light;
   border-color: $primary-color;
  }
 }
}

.posts-grid {
 display: grid;
 grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
 gap: $spacing-lg;
 margin-bottom: $spacing-xl;

 @media (max-width: $breakpoint-mobile) {
  grid-template-columns: 1fr;
  gap: $spacing-md;
 }
}

.post-card {
 background-color: white;
 border-radius: $border-radius;
 overflow: hidden;
 box-shadow: 0 2px 8px $shadow;
 cursor: pointer;
 transition: $transition;
 animation: fadeIn 0.5s ease;

 &:hover {
  transform: translateY(-8px);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.15);

  .post-image img {
   transform: scale(1.05);
  }
 }

 .post-image {
  position: relative;
  height: 240px;
  overflow: hidden;

  img {
   width: 100%;
   height: 100%;
   object-fit: cover;
   transition: $transition;
  }

  .post-category {
   position: absolute;
   top: $spacing-md;
   right: $spacing-md;
   background-color: $primary-color;
   color: $text-light;
   padding: $spacing-xs $spacing-sm;
   border-radius: $border-radius;
   font-size: 0.875rem;
   font-weight: 600;
  }
 }

 .post-content {
  padding: $spacing-md;

  h3 {
   color: $text-dark;
   margin-bottom: $spacing-sm;
   font-size: 1.25rem;
   line-height: 1.4;
  }

  .post-description {
   color: #666;
   margin-bottom: $spacing-md;
   font-size: 0.95rem;
   line-height: 1.6;
   display: -webkit-box;
   -webkit-line-clamp: 3;
   line-clamp: 3;
   -webkit-box-orient: vertical;
   overflow: hidden;
  }

  .post-meta {
   display: flex;
   justify-content: space-between;
   align-items: center;
   font-size: 0.875rem;
   color: #999;
   padding-top: $spacing-sm;
   border-top: 1px solid $secondary-color;

   .post-author {
    font-weight: 500;
   }
  }
 }
}

.load-more-section {
 text-align: center;
 padding-top: $spacing-lg;

 .load-more-btn {
  padding: $spacing-sm $spacing-xl;
  background-color: $primary-color;
  color: $text-light;
  border: none;
  border-radius: $border-radius;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: $transition;

  &:hover {
   background-color: color.adjust(#c41e3a, $lightness: -10%);
   transform: translateY(-2px);
   box-shadow: 0 4px 12px $shadow;
  }
 }
}
</style>
