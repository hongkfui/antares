<template>
   <BaseContextMenu
      :context-event="contextEvent"
      @close-context="$emit('close-context')"
   >
      <div class="context-element" @click="showEditModal(contextConnection)">
         <i class="mdi mdi-18px mdi-pencil text-light pr-1" /> {{ $t('word.edit') }}
      </div>
      <div class="context-element" @click="showConfirmModal">
         <i class="mdi mdi-18px mdi-delete text-light pr-1" /> {{ $t('word.delete') }}
      </div>

      <ConfirmModal
         v-if="isConfirmModal"
         @confirm="deleteConnection(contextConnection)"
         @hide="hideConfirmModal"
      >
         <template :slot="'header'">
            <div class="d-flex">
               <i class="mdi mdi-24px mdi-server-remove mr-1" /> {{ $t('message.deleteConnection') }}
            </div>
         </template>
         <div :slot="'body'">
            <div class="mb-2">
               {{ $t('message.deleteConnectionCorfirm') }} <b>{{ connectionName }}</b>?
            </div>
         </div>
      </ConfirmModal>
   </BaseContextMenu>
</template>

<script>
import { mapActions, mapGetters } from 'vuex';
import BaseContextMenu from '@/components/BaseContextMenu';
import ConfirmModal from '@/components/BaseConfirmModal';

export default {
   name: 'SettingBarContext',
   components: {
      BaseContextMenu,
      ConfirmModal
   },
   props: {
      contextEvent: MouseEvent,
      contextConnection: Object
   },
   data () {
      return {
         isConfirmModal: false
      };
   },
   computed: {
      ...mapGetters({
         getConnectionName: 'connections/getConnectionName'
      }),
      connectionName () {
         return this.getConnectionName(this.contextConnection.uid);
      }
   },
   methods: {
      ...mapActions({
         deleteConnection: 'connections/deleteConnection',
         showEditModal: 'application/showEditConnModal'
      }),
      showConfirmModal () {
         this.isConfirmModal = true;
      },
      hideConfirmModal () {
         this.isConfirmModal = false;
      }
   }
};
</script>
