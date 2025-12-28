<script setup>
import { ref, watch } from "vue";

const props = defineProps({
  isOpen: {
    type: Boolean,
    default: false,
  },
});

const emit = defineEmits(["close"]);
const dialogRef = ref(null);

const syncDialog = (open) => {
  const dialog = dialogRef.value;
  // if (!dialog) return;
  // open ? dialog.showModal() : dialog.close();
  if (open) {
    if (!dialog.open) dialog.showModal();
  } else {
    if (dialog.open) dialog.close();
  }
};

watch(
  () => props.isOpen,
  (newVal) => {
    syncDialog(newVal);
  }
  // { immediate: true, flush: "post" }
);

const close = () => emit("close", false);
</script>

<template>
  <Teleport to="#modal">
    <dialog ref="dialogRef" @cancel.prevent="close" @close="close">
      <slot name="header">
        <div class="default-header">
          <div class="title">Default Title</div>
          <button class="close-button" @click="close">X</button>
        </div>
      </slot>
      <slot name="body">
        <div class="default-body">Default Body Content</div>
      </slot>
      <slot name="footer">
        <div class="default-footer">
          <button @click="close">Close</button>
        </div>
      </slot>
    </dialog>
  </Teleport>
</template>

<style scoped>
dialog::backdrop {
  background: rgba(0, 0, 0, 0.5);
}
</style>
