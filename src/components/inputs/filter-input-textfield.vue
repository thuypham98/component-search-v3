<script setup lang="ts">
import { FilterInput } from '@/components'
import useFilterInput from '@/composables/filter-input'
import { TextfieldOptions } from '@/types';

const { inputValue, internalValue, filter } = useFilterInput(inject('storeId')!)
const options = computed(
  () => filter.value.options?.inputTextfield || {}
) as ComputedRef<TextfieldOptions>

onMounted(() => {
  setTimeout(autoFocusInput, 200)
})

function autoFocusInput() {
  const input = document.querySelector(
    '.filter-input-textfield input'
  ) as HTMLInputElement
  if (input) input.focus()
}
</script>

<template>
  <filter-input v-bind="{ internalValue }" class="-textfield">
    <div
      :class="['filter-input-textfield', { '-fullwidth': options.fullwidth }]"
    >
      <v-form class="form-field">
        <v-text-field
          v-model="inputValue"
          class="textfield"
          :label="filter.label"
          :maxlength="options.maxlength"
          :counter="options.countable"
        >
          <template #prepend v-if="options.preText">
            <span class="pre">{{ options.preText }}</span>
          </template>
          <template #append v-if="options.postText">
            <span class="post">{{ options.postText }}</span>
          </template>
        </v-text-field>
      </v-form>
    </div>
  </filter-input>
</template>

<style lang="scss" scoped>
.filter-input-textfield {
  display: flex;
  align-items: center;
  > .pre {
    margin-right: 10px;
  }
  > .post {
    margin-left: 10px;
  }
  &.-fullwidth {
    width: 100%;
  }
}
.form-field {
  width: 100%;
  > .textfield {
    grid-template-columns: max-content minmax(200px, 1fr) max-content;
  }
}
</style>
