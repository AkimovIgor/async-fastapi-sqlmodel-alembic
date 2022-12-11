<template>
  <v-dialog v-model="dialog" :value="visible" persistent max-width="450">
    <v-card>
      <v-card-title v-text="title" />
      <v-card-text v-text="message" />
      <v-card-actions v-if="visible">
        <template v-for="action in value">
          <v-spacer :key="action.label" v-if="typeof action == 'string'" />
          <v-btn
            v-else
            text
            :key="action.label"
            v-text="action.label"
            @click="doAction(action.action)"
            :color="action.color"
          />
        </template>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>
<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component';
import { Prop, Watch } from 'vue-property-decorator';
@Component
export default class DialogConfirm extends Vue {

  @Prop({ type: Boolean, default: false })
  dialog: boolean | undefined

  @Prop({ type: String, default: "Подтверждение удаления" })
  title: string | undefined

  @Prop({ type: String, default: "Вы уверены, что хотите удалить?" })
  message: string | undefined

  @Prop({ type: Array, default: undefined })
  value: { label: string, action: () => boolean, color: string }[] | undefined

  get visible() {
    return Array.isArray(this.value) && this.value.length > 0
  }

  doAction(action: () => boolean) {
    if ('undefined' == typeof action || action()) {
      this.$emit('input', false);
    }
  }
}
</script>