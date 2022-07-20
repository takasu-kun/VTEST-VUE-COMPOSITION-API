<script setup lang="ts">
import { provide } from "vue";
import useToast from "./composables/useToast";
import ToastProvider from "./components/ToastProvider.vue";

const {
  notifications,
  createNotification,
  removeNotifications,
  stopBodyOverflow,
  allowBodyOverflow,
} = useToast();

provide("create-notification", createNotification);
</script>

<template>
  <div class="main flex items-center justify-center gap-4 w-screen h-screen">
     <div class="btn-group flex flex-col items-center justify-center gap-4">
      <button class="p-4 bg-indigo-100 border border-solid border-indigo-500 rounded-md"
        @click.prevent="createNotification({ 
          message: 'This is an info toast' 
          })"
      >
       Show Info Toast
      </button>
      <button class="p-4 bg-indigo-100 border border-solid border-indigo-500 rounded-md"
        @click.prevent="
          createNotification({
            type: 'success',
            message: 'This is a success toast',
          })
        "
      >
        Show Success Toast
      </button>
      <button class="p-4 bg-indigo-100 border border-solid border-indigo-500 rounded-md"
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
    <transition-group
      name="toast-provider"
      tag="div"
      class="toast-providers"
      @before-enter="stopBodyOverflow"
      @after-enter="allowBodyOverflow"
      @before-leave="stopBodyOverflow"
      @after-leave="allowBodyOverflow"
    >
      <toast-provider
        v-for="(item, idx) in notifications"
        :key="item.id"
        :id="item.id"
        :type="item.type"
        :title="item.title"
        :message="item.message"
        :auto-close="item.autoClose"
        :duration="item.duration"
        @close="
          () => {
            removeNotifications(item.id);
          }
        "
      ></toast-provider>
    </transition-group>
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
