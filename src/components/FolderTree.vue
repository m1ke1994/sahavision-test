<template>
  <ul>
    <li v-for="item in data" :key="item.id">
      <div @click="toggle(item)" :class="{ 'selected': item.selected }">
        <span v-if="item.children" :class="{ 'expanded': item.expanded }">
          {{ item.name }}
        </span>
        <span v-else @click="select(item)">
          {{ item.name }}
        </span>
      </div>
      <FolderTree v-if="item.children && item.expanded" :data="item.children" @select="select" />
    </li>
  </ul>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';

interface Folder {
  id: number;
  name: string;
  children?: Folder[];
  expanded?: boolean;
  selected?: boolean;
}

export default defineComponent({
  name: 'FolderTree',
  props: {
    data: {
      type: Array as PropType<Folder[]>,
      required: true,
    },
  },
  emits: ['select'],
  setup(props, { emit }) {
    const toggle = (item: Folder) => {
      if (item.children) {
        item.expanded = !item.expanded;
      }
    };

    const select = (item: Folder) => {
      emit('select', item);
    };

    return {
      toggle,
      select,
    };
  },
});
</script>

<style scoped>
ul {
  list-style-type: none;
  padding-left: 20px;
}

li {
  margin: 5px 0;
}

.selected {
  background-color: #e0e0e0;
}

.expanded::before {
  content: '▼';
  margin-right: 5px;
}
</style>