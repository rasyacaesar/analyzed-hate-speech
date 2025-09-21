<template>
  <div 
    class="spinner" 
    :class="{ 'spinner--small': size === 'small', 'spinner--large': size === 'large' }"
    :style="{ borderTopColor: color, borderRightColor: color }"
  ></div>
</template>

<script>
export default {
  name: 'SpinnerComponent',
  props: {
    size: {
      type: String,
      default: 'medium',
      validator: (value) => ['small', 'medium', 'large'].includes(value)
    },
    color: {
      type: String,
      default: '#3498db'
    }
  }
}
</script>

<style scoped>
.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #f3f3f3;
  border-top: 4px solid #3498db;
  border-right: 4px solid #3498db;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  display: inline-block;
}

.spinner--small {
  width: 20px;
  height: 20px;
  border-width: 2px;
}

.spinner--large {
  width: 60px;
  height: 60px;
  border-width: 6px;
}

@keyframes spin {
  0% { 
    transform: rotate(0deg); 
  }
  100% { 
    transform: rotate(360deg); 
  }
}
</style>