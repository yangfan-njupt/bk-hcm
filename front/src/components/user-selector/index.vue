<script setup lang="ts">
import { computed } from 'vue';
import BkUserSelector from '@blueking/bk-user-selector';
import '@blueking/bk-user-selector/vue3/vue3.css';

import { useUserStore } from '@/store/user';

defineOptions({ name: 'user-selector' });

const model = defineModel<string | string[]>();

const props = withDefaults(defineProps<IUserSelectorProps>(), {
  multiple: true,
  allowCreate: true,
  hasDeleteIcon: true,
  clearable: true,
  trigger: 'focus',
  collapseTags: true,
  placeholder: '请输入',
});

export interface IUserSelectorProps {
  multiple?: boolean;
  disabled?: boolean;
  clearable?: boolean;
  placeholder?: string;
  fastSelect?: boolean;
  allowCreate?: boolean;
}

const props = withDefaults(defineProps<IUserSelectorProps>(), {
  multiple: true,
  allowCreate: true,
  clearable: true,
  placeholder: '请输入',
  fastSelect: true,
});

const emit = defineEmits<{
  change: [val: string | string[]];
}>();

const userStore = useUserStore();

const tenantId = computed(() => userStore.tenantId);
const currentUserId = computed(() => props.fastSelect && userStore.username);
const apiBaseUrl = window.PROJECT_CONFIG.USER_MANAGE_URL;

const handleChange = (val: string | string[]) => {
  emit('change', val);
};

const focus = () => {
  tagInputRef.value?.focusInputTrigger?.();
};

defineExpose({
  getValue() {
    if (tagInputRef.value?.getValue) {
      return tagInputRef.value.getValue().then(() => model.value);
    }
    return model.value;
  },
  focus,
});
</script>

<template>
  <bk-user-selector
    class="user-selector"
    v-model="model"
    :multiple="multiple"
    :placeholder="placeholder"
    :tenant-id="tenantId"
    :current-user-id="currentUserId"
    :api-base-url="apiBaseUrl"
    @change="handleChange"
  />
</template>

<style lang="scss" scoped></style>
