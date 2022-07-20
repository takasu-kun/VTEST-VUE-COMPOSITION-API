<script setup lang="ts">
import { computed, onMounted, ref } from "vue";


const props = defineProps({
  id: { type: String, required: true },
  type: {
    type: String,
    default: "info",
    required: false,
  },
  title: { type: String, default: null, required: false },
  message: {
    type: String,
    default: "--.",
    required: false,
  },
  autoClose: { type: Boolean, default: true, required: false },
  duration: { type: Number, default: 5, required: false },
});

const emit = defineEmits<{
  (e: "close"): void;
}>();

const timer = ref(-1);
const startedAt = ref<number>(0);
const delay = ref<number>(0);

onMounted(() => {
  if (props.autoClose) {
    startedAt.value = Date.now();
    delay.value = props.duration * 1000;
    timer.value = setTimeout(close, delay.value);
  }
});

const toastIcon = computed(() => {
  switch (props.type) {
    case "error":
      return "ri-emotion-unhappy-line";
    case "warning":
      return "ri-error-warning-line";
    case "success":
      return "ri-emotion-happy-line";
    default:
      return "ri-information-line";
  }
});

const toastColor = computed(() => {
  switch (props.type) {
    case "error":
      return "border-red-500 bg-red-100";
    case "success":
      return "border-green-500 bg-green-100";
    default:
      return "border-indigo-500 bg-indigo-100";
  }
});


const toastTitle = computed(() => {
  return props.title && props.title !== null ? props.title : "Notification";
});


const close = () => {
  emit("close");
};
</script>

<template>
  <div
    :class="`${toastColor} toast-notification p-3 rounded-lg border border-solid w-full list-leave-active list-move list-leave-to`"
    :style="`--toast-duration: ${duration}s;`"
    @click.prevent="close"
    :ref="id"
  >
      <div class="content">
        <p class="content__message">{{ message }}</p>
      </div>
  </div>
</template>

<style lang="scss" scoped>
  @keyframes progress {
    to {
      width: 0;
    }
  }

  @keyframes toast-fade-in {
    to {
      opacity: 1;
    }
  }

  @keyframes toast-fade-out {
    from {
      opacity: 1;
    }

    to {
      opacity: 0;
    }
  }

</style>
