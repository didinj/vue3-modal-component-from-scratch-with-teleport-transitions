<template>
  <!-- Mount outside the parent using Teleport -->
  <Teleport to="body">
    <Transition name="modal-fade">
      <!-- Only render if visible -->
      <div
        v-if="props.show"
        class="modal-overlay"
        :class="{ 'overlay-blur': props.blurOverlay }"
        @click="handleOverlayClick"
      >
        <div
            class="modal-container"
            :class="[
                `size-${props.size}`,
                `position-${props.position}`,
                props.panelMode ? 'panel-mode' : '',
                `anim-${props.animation}`,
            ]"
            :style="{
                maxWidth: props.width || '',
                borderRadius: props.rounded
            }"
            ref="modalRef"
            role="dialog"
            aria-modal="true"
            :aria-labelledby="headerId"
            :aria-describedby="bodyId"
            @click.stop
        >
            <button
                v-if="props.showCloseButton"
                class="modal-close"
                @click="close"
                aria-label="Close modal"
            >
                ×
            </button>
          <!-- Header -->
          <header v-if="$slots.header" :id="headerId" class="modal-header">
            <slot name="header"></slot>
          </header>

          <!-- Body -->
          <section :id="bodyId" class="modal-body">
            <slot></slot>
          </section>

          <!-- Footer -->
          <footer v-if="$slots.footer" class="modal-footer">
            <slot name="footer"></slot>
          </footer>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup lang="ts">
import { ref, watch, onMounted, onBeforeUnmount } from 'vue'

const props = defineProps({
  show: { type: Boolean, default: false },
  closeOnEsc: { type: Boolean, default: true },
  closeOnClickOutside: { type: Boolean, default: true },

  /* === Enhancement Props === */
  size: {
    type: String,
    default: "md", // sm, md, lg, full, or custom
  },
  position: {
    type: String,
    default: "center", // center or top
  },
  showCloseButton: {
    type: Boolean,
    default: false,
  },
  width: {
    type: String,
    default: "", // e.g., "400px" or "50%"
  },
  rounded: {
    type: String,
    default: "12px", // border radius
  },
  animation: {
    type: String,
    default: "fade", 
    validator: (v: string) =>
        [
        "fade",
        "scale",
        "slide-up",
        "slide-down",
        "slide-left",
        "slide-right"
        ].includes(v)
  },
  blurOverlay: {
    type: Boolean,
    default: false
  },
  panelMode: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['update:show'])

const modalRef = ref<HTMLElement | null>(null)

const headerId = `modal-header-${Math.random().toString(36).slice(2)}`
const bodyId = `modal-body-${Math.random().toString(36).slice(2)}`

/**
 * Close modal
 */
const close = () => emit('update:show', false)

/**
 * Click outside the modal container
 */
const handleOverlayClick = () => {
  if (props.closeOnClickOutside) close()
}

/**
 * ESC key listener
 */
const handleKeydown = (e: KeyboardEvent) => {
  if (e.key === 'Escape' && props.closeOnEsc) {
    close()
  }
}

/**
 * Manage event listeners
 */
onMounted(() => {
  window.addEventListener('keydown', handleKeydown)
})

onBeforeUnmount(() => {
  window.removeEventListener('keydown', handleKeydown)
})

/**
 * Focus trap (simple version)
 */
watch(
  () => props.show,
  (visible) => {
    if (visible) {
      // Give Vue time to render
      setTimeout(() => modalRef.value?.focus(), 10)
    }
  }
)
</script>

<style scoped>
/* Overlay */
.modal-overlay {
  position: fixed;
  inset: 0;
  background: var(--overlay-bg);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1rem;
  z-index: 999;
}

/* Modal */
.modal-container {
  background: var(--modal-bg);
  max-width: var(--modal-max-width);
  width: 100%;
  border-radius: var(--modal-radius);
  padding: 1.5rem;
  outline: none;
  transform-origin: center;
}

/* Sections */
.modal-header { margin-bottom: 1rem; }
.modal-body { margin-bottom: 1rem; }
.modal-footer { text-align: right; }

/* Transitions */
.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: opacity 0.25s ease, transform 0.25s ease;
}
.modal-fade-enter-from,
.modal-fade-leave-to {
  opacity: 0;
  transform: scale(0.95);
}

/* Close Button */
.modal-close {
  position: absolute;
  top: 0.75rem;
  right: 0.75rem;
  background: transparent;
  border: none;
  font-size: 1.6rem;
  cursor: pointer;
  line-height: 1;
  color: #555;
}
.modal-close:hover {
  color: #111;
}

/* Position Variants */
.position-top .modal-container {
  margin-top: 5vh;
}
.position-center .modal-container {
  margin-top: 0;
}

/* Size Variants */
.size-sm { max-width: 350px !important; }
.size-md { max-width: 500px !important; }
.size-lg { max-width: 800px !important; }
.size-full { max-width: 95vw !important; }

/* Animation Variants */
.anim-fade.modal-fade-enter-from,
.anim-fade.modal-fade-leave-to {
  opacity: 0;
}

.anim-scale.modal-fade-enter-from,
.anim-scale.modal-fade-leave-to {
  opacity: 0;
  transform: scale(0.9);
}

.anim-slide-up.modal-fade-enter-from,
.anim-slide-up.modal-fade-leave-to {
  opacity: 0;
  transform: translateY(20px);
}

/* Spring Scale */
.anim-scale.modal-fade-enter-active,
.anim-scale.modal-fade-leave-active {
  transition: opacity 0.3s ease, transform 0.35s cubic-bezier(.18,.89,.32,1.28);
}

.anim-scale.modal-fade-enter-from,
.anim-scale.modal-fade-leave-to {
  opacity: 0;
  transform: scale(0.85);
}

/* Slide Up */
.anim-slide-up.modal-fade-enter-from,
.anim-slide-up.modal-fade-leave-to {
  opacity: 0;
  transform: translateY(30px);
}

/* Slide Down */
.anim-slide-down.modal-fade-enter-from,
.anim-slide-down.modal-fade-leave-to {
  opacity: 0;
  transform: translateY(-30px);
}

/* Slide Left */
.anim-slide-left.modal-fade-enter-from,
.anim-slide-left.modal-fade-leave-to {
  opacity: 0;
  transform: translateX(40px);
}

/* Slide Right */
.anim-slide-right.modal-fade-enter-from,
.anim-slide-right.modal-fade-leave-to {
  opacity: 0;
  transform: translateX(-40px);
}

/* Slide Panel Base */
.panel-mode {
  width: 100%;
  max-width: none;
  height: auto;
  border-radius: 0;
  padding: 1.25rem;
}

/* Slide Left Panel */
.panel-mode.anim-slide-left {
  align-self: stretch;
  margin-left: 0;
  height: 100%;
}

/* Slide Right Panel */
.panel-mode.anim-slide-right {
  align-self: stretch;
  margin-right: 0;
  height: 100%;
}

/* Bottom Sheet */
.panel-mode.anim-slide-up {
  margin-bottom: 0;
  width: 100%;
  border-radius: 20px 20px 0 0;
  position: fixed;
  bottom: 0;
  left: 0;
}

</style>
