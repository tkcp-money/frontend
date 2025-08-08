<script setup lang="ts">
  import * as z from 'zod'
  import type { FormSubmitEvent } from '@nuxt/ui'
  import { computed, reactive, watch } from 'vue'

  const props = defineProps<{ modelValue: boolean }>()
  const emit = defineEmits<{ (e: 'update:modelValue', value: boolean): void }>()

  const model = computed({
    get: () => props.modelValue,
    set: (val: boolean) => emit('update:modelValue', val),
  })

  watch(
      () => model.value,
      (val: boolean) => {
        const color = val ? 'white' : ''

        window.ReactNativeWebView?.postMessage(
            JSON.stringify({ type: 'openModal', color: color })
        );
      });

  // Валюты для выбора
  const currencies = [
    { label: 'USD', value: 'USD' },
    { label: 'EUR', value: 'EUR' },
    { label: 'RUB', value: 'RUB' },
    { label: 'KZT', value: 'KZT' },
    { label: 'UAH', value: 'UAH' },
    { label: 'GBP', value: 'GBP' },
    { label: 'CNY', value: 'CNY' },
    { label: 'JPY', value: 'JPY' },
    { label: 'CHF', value: 'CHF' },
    { label: 'CAD', value: 'CAD' },
    { label: 'AUD', value: 'AUD' },
    { label: 'SEK', value: 'SEK' },
    { label: 'NOK', value: 'NOK' },
    { label: 'PLN', value: 'PLN' },
    { label: 'TRY', value: 'TRY' },
    { label: 'INR', value: 'INR' },
  ]

  // Цвета банков (примерная палитра)
  const bankColors = [
    '#4f46e5', // фиолетовый (Tinkoff)
    '#1a1f71', // синий (Visa)
    '#e60000', // красный (Альфа)
    '#009639', // зеленый (Сбер)
    '#ffdd2d', // желтый (Райффайзен)
    '#0057b8', // синий (ВТБ)
    '#f36f21', // оранжевый (Росбанк)
    '#6c757d', // серый (Газпромбанк)
    '#00b1e7', // голубой (Открытие)
    '#e30613', // красный (МТС)
    '#b41c18', // бордовый (ПСБ)
    '#f58220', // оранжевый (Home Credit)
    '#2d2926', // черный (Юникредит)
    '#8dc63f', // салатовый (Ак Барс)
    '#7f3f98', // фиолетовый (Росбанк)
  ]

  // Схема валидации
  const schema = z.object({
    institution: z.string().min(2, 'Слишком коротко'),
    accountName: z.string().min(2, 'Слишком коротко'),
    currency: z.string().min(1, 'Выберите валюту'),
    balance: z.coerce.number().min(0, 'Баланс не может быть отрицательным'),
    color: z.string().min(1, 'Выберите цвет'),
  })

  type Schema = z.output<typeof schema>

  const state = reactive<Partial<Schema>>({
    institution: '',
    accountName: '',
    currency: 'USD',
    balance: 0,
    color: '#4f46e5',
  })

  const toast = useToast()
  async function onSubmit(event: FormSubmitEvent<Schema>) {
    toast.add({ title: 'Счет добавлен', description: `Счет ${event.data.accountName} (${event.data.institution}) создан`, color: 'success' })
    model.value = false
  }
</script>

<template>
  <UModal v-model:open="model" title="Добавить счет" description="Создайте новый счет (аккаунт)" fullscreen :ui="{
          footer: 'modal-footer',
        }">
    <template #body>
      <!-- Карта визуализации -->
      <div class="mb-6 flex justify-center">
        <div
          class="rounded-xl shadow-lg p-6 w-80 h-44 flex flex-col justify-between"
          :style="{ background: state.color, color: '#fff', transition: 'background 0.2s' }"
        >
          <div class="flex justify-between items-center">
            <span class="text-lg font-bold">{{ state.institution || 'Название банка' }}</span>
            <span class="text-sm">{{ state.currency }}</span>
          </div>
          <!-- <div class="flex-1 flex flex-col justify-center items-center">
            <span class="text-2xl font-semibold"></span>
          </div> -->
          <div class="flex justify-between items-end">
            <span class="text-base">{{ state.accountName || 'Название счета' }}</span>
            <!-- <span class="text-xl font-bold">{{ state.balance?.toLocaleString() || '0' }}</span> -->
          </div>
        </div>
      </div>
      <UForm
          :schema="schema"
          :state="state"
          class="space-y-4"
          @submit="onSubmit"
      >
        <UFormField label="Название банка/институции" name="institution">
          <UInput v-model="state.institution" class="w-full" placeholder="Например, Сбербанк" />
        </UFormField>
        <UFormField label="Название счета" name="accountName">
          <UInput v-model="state.accountName" class="w-full" placeholder="Мой счет" />
        </UFormField>
        <UFormField label="Валюта" name="currency">
          <USelect v-model="state.currency" :items="currencies" class="w-full" />
        </UFormField>
        <UFormField label="Баланс" name="balance">
          <UInput v-model="state.balance" type="number" class="w-full" placeholder="0" />
        </UFormField>
        <UFormField label="Цвет карты" name="color">
          <div class="flex flex-wrap gap-2">
            <div
              v-for="color in bankColors"
              :key="color"
              :style="{
                background: color,
                width: '2rem',
                height: '2rem',
                borderRadius: '0.375rem',
                border: state.color === color ? '3px solid #000' : '2px solid #fff',
                boxShadow: state.color === color ? '0 0 0 2px #6366f1' : '0 0 0 1px #e5e7eb',
                cursor: 'pointer',
                transition: 'border 0.2s, box-shadow 0.2s',
              }"
              @click="state.color = color"
              :aria-label="`Выбрать цвет ${color}`"
              tabindex="0"
            />
          </div>
        </UFormField>
        <div class="flex justify-end gap-2">
          <UButton
              label="Отмена"
              color="neutral"
              variant="subtle"
              @click="model = false"
          />
          <UButton
              label="Создать"
              color="primary"
              variant="solid"
              type="submit"
              style=""
          />
        </div>
      </UForm>
    </template>
    <template #footer>
      <UButton label="Submit" color="neutral" style="width: 100%"/>
    </template>
  </UModal>
</template>

<style lang="scss">
  .modal-footer {
    padding: 0!important;
  }
</style>