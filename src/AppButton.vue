<template>
   <button 
   class="btn" 
   :class="color" 
   :disabled="disabledButton"
   @click.prevent="$emit(action)"><slot></slot></button>
</template>

<script>
export default {
  emits: ['upload-comments', 'add-new'],
  props: {
    color: {
      type: String,
      default: 'primary',
    },
    action: {
      type: String,
      required: true,
    },
    disabledButton: {
      default: false,
      type: Boolean,
    }
  }

}
</script>

<style>

</style>