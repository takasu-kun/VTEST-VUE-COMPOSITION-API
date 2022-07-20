<script setup lang="ts">

import { inject } from "vue";
import { CreateNotification } from "./composables/useToast";

const createNotification = <CreateNotification>inject("create-notification");

</script>

<template>
  <div class="main flex items-center justify-center gap-4 w-screen h-screen">
     <div class="btn-group flex flex-col items-center justify-center gap-4">
      <button class="uppercase p-4 bg-indigo-100 border border-solid border-indigo-500 rounded-md"
        @click.prevent="createNotification({ 
          message: 'This is an info toast' 
          })"
      >
       Show Info Toast
      </button>
      <button class="uppercase p-4 bg-indigo-100 border border-solid border-indigo-500 rounded-md"
        @click.prevent="
          createNotification({
            type: 'success',
            message: 'This is a success toast',
          })
        "
      >
        Show Success Toast
      </button>
      <button class="uppercase p-4 bg-indigo-100 border border-solid border-indigo-500 rounded-md"
        @click.prevent="
          createNotification({
            type: 'error',
            message: 'This is an error toast',
          })
        "
      >
        Show Error Toast
      </button>
    </div>
  </div>
</template>

<style lang="scss">
.toast-providers {
    z-index: 100;
    position: absolute;
    top: 0.5rem;
    right: 0.5rem;
    display: flex;
    flex-direction: column-reverse;
    gap: 0.8rem;
  }

  .toast-provider-enter-active {
    animation: toast-fade-in 0.5s ease-in-out;
  }
  .toast-provider-leave-active {
    animation: toast-fade-in 0.5s ease-in-out reverse;
  }

  @keyframes toast-fade-in {
    from {
      opacity: 0;
      transform: scale(0.4);
    }
    to {
      opacity: 1;
      transform: scale(1);
    }
  }
</style>
