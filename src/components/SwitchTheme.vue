<template>
  <arco-tooltip content="切换主题" class="custom-tooltip">
    <div class="global-icon" @click="toggleTheme">
      <span>{{ currentIcon }}</span>
    </div>
  </arco-tooltip>
</template>

<script>
import { ref, watchEffect, onMounted } from 'vue';
import { Tooltip } from '@arco-design/web-vue';

export default {
  name: 'SwitchTheme',
  components: {
    'arco-tooltip': Tooltip,
  },
  setup() {
    const theme = ref(localStorage.getItem('userTheme') || 'light');
    const currentIcon = ref(theme.value === 'light' ? '🌞' : '🌙');

    const setTheme = (themeValue) => {
      document.body.setAttribute('arco-theme', themeValue === 'dark' ? 'dark' : '');
      localStorage.setItem('userTheme', themeValue);
    };

    const toggleTheme = () => {
      theme.value = theme.value === 'light' ? 'dark' : 'light';
      setTheme(theme.value);
    };

    watchEffect(() => {
      currentIcon.value = theme.value === 'light' ? '🌞' : '🌙';
    });

    onMounted(() => {
      setTheme(theme.value);
    });

    return {
      currentIcon,
      toggleTheme,
    };
  },
};
</script>

<style scoped>
.global-icon {
  font-size: 28px;
  position: fixed;
  bottom: 20px;
  right: 20px;
  z-index: 1000;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 50px;
  height: 50px;
  background-color: var(--color-bg-container);
  border-radius: 50%;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
  transition: background-color 0.3s, transform 0.3s;
}

.global-icon:hover {
  background-color: var(--color-bg-hover);
  transform: scale(1.1);
}

.global-icon span {
  color: var(--color-text);
}

.custom-tooltip .arco-tooltip-content {
  background-color: var(--color-bg-container);
  color: var(--color-text);
  border-radius: 8px;
  padding: 8px 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  font-size: 14px;
  border: 1px solid var(--color-border);
}

.custom-tooltip .arco-tooltip-arrow {
  color: var(--color-bg-container);
}
</style>
