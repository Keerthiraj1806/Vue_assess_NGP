<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>
    <div class="main" style="width:25%">
      <div>
        <nav class="sidenav">
          <RouterLink class= 'route' to="/">Home</RouterLink><br>
          <RouterLink class= 'route' to="/about">About</RouterLink><br>
        </nav>
      </div>
    </div>
  </header>

  <RouterView style="width: 75%;"></RouterView>
</template>

<style scoped>
        /* Style for the container */
        .main {
            display: flex;
            height: 100vh;
        }
        

        /* Style for the sidebar */
        .sidenav {
                height: 100%;
                width: 200px;
                position: fixed;
                top: 10px;
                left: 10px;
                /* Add padding and background color */
                padding: 20px;
                background-color: #f0f0f0;
                /* Add box-shadow for depth */
                box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
        }

        /* Style for the sidebar links */
        .route {
              font-size: 16px;
              display: block;
              color: #333;
              text-decoration: none;
              margin-bottom: 10px;
              padding: 8px 12px;
              border-radius: 4px;
              transition: background-color 0.3s;
          }

          .route:hover {
              background-color: #e6e6e6;
          }

          .route.active {
              color: #fff;
              background-color: #007bff;
              font-weight: bold;
          }
</style>
