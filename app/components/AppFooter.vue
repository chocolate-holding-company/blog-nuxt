<template>
 <footer class="footer">
  <div class="container">
   <div class="footer-content">
    <div class="footer-section">
     <h3>Hill Walking Made Easy</h3>
     <p>Your guide to exploring the hills safely and confidently.</p>
    </div>
   </div>
   <div class="footer-bottom">
    <p>
     &copy; {{ currentYear }} Hill Walking Made Easy |
     <a href="#" @click.prevent="$emit('openContact')">Contact Us</a>
    </p>
   </div>
  </div>
 </footer>
</template>

<script setup>
const currentYear = new Date().getFullYear();
defineEmits(["openContact"]);
</script>

<style lang="scss" scoped>
.footer {
 background-color: $text-dark;
 color: $text-light;
 padding: $spacing-xl 0 $spacing-md;
 margin-top: $spacing-xxl;

 .footer-content {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: $spacing-lg;
  margin-bottom: $spacing-lg;

  @media (max-width: $breakpoint-mobile) {
   grid-template-columns: 1fr;
   gap: $spacing-md;
  }
 }

 .footer-section {
  h3 {
   color: $primary-color;
   margin-bottom: $spacing-sm;
   font-size: 1.25rem;
  }

  h4 {
   color: $text-light;
   margin-bottom: $spacing-sm;
   font-size: 1.1rem;
  }

  p {
   margin-bottom: $spacing-sm;
   opacity: 0.9;
  }

  ul {
   list-style: none;
   padding: 0;

   li {
    margin-bottom: $spacing-xs;

    a {
     color: $text-light;
     opacity: 0.9;
     transition: $transition;

     &:hover {
      opacity: 1;
      color: $primary-color;
     }
    }
   }
  }
 }

 .footer-bottom {
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  padding-top: $spacing-md;
  text-align: center;

  p {
   margin: 0;
   opacity: 0.8;
   font-size: 0.9rem;
  }
 }
}
</style>
