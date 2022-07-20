<template>
    <App />
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
</template>

<script setup lang="ts">
import App from './App.vue';
import ToastProvider from "./components/ToastProvider.vue";
import useToast from "./composables/useToast";
import { provide } from "vue";

const {
  notifications,
  createNotification,
  removeNotifications,
  stopBodyOverflow,
  allowBodyOverflow,
} = useToast();

provide("create-notification", createNotification);
</script>
