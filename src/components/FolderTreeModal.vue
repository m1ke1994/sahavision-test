<template>
  <div class="flex flex-col p-10">

    <button
      class="px-4 py-2 border border-black w-48 mx-auto bg-white hover:shadow-2xl hover:bg-gray-300 transition-all duration-100 "
      type="button" @click="showModal = true">Открыть</button>


    <Modal :title="modalTitle" :visible="showModal" @close="showModal = false" @confirm="handleConfirm">
      <FolderTree :data="folderData" @select="handleSelect" />
    </Modal>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, reactive } from 'vue';
import Modal from './Modal.vue';
import FolderTree from './FolderTree.vue';

interface Folder {
  id: number;
  name: string;
  children?: Folder[];
  expanded?: boolean;
  selected?: boolean;
}

export default defineComponent({
  components: {
    Modal,
    FolderTree,
  },
  emits: ['select'],
  setup(props, { emit }) {
    const showModal = ref(false);
    const modalTitle = ref('Дерево папок');
    const selectedFolder = ref<Folder | null>(null);

    const folderData: Folder[] = reactive([
      {
        id: 1,
        name: 'Папка 1',
        children: [
          { id: 2, name: 'Файл 1.1' },
          { id: 3, name: 'Файл 1.2' },
        ],
      },
      {
        id: 4,
        name: 'Папка 2',
        children: [
          { id: 5, name: 'Файл 2.1' },
          {
            id: 6,
            name: 'Папка 2.2',
            children: [
              { id: 7, name: 'Файл 2.2.1' },
            ],
          },
        ],
      },
    ]);

    const handleSelect = (folder: Folder) => {
      selectedFolder.value = folder;
    };

    const handleConfirm = () => {
      if (selectedFolder.value) {
        emit('select', selectedFolder.value.id);
      }
      showModal.value = false;
    };

    return {
      showModal,
      modalTitle,
      folderData,
      handleSelect,
      handleConfirm,
    };
  },
});
</script>

<style scoped>
button {
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}
</style>