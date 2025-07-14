<script setup lang="ts">
import containerStyle from '@/shared/containerShared/containerStyle.vue'
import { computed, onMounted, reactive, ref, watch } from 'vue'

onMounted(() => {
  const saveItemStep = localStorage.getItem('stepValue')
  if (saveItemStep) {
    CurrentStep.value = Number(saveItemStep)
  }
})

const title = 'План по изучению Vue.js'

const ItemsStep = reactive([
  { id: 1, label: 'Основной', labelText: 'Здесь ты узнаешь о VUE 3 Composition API' },
  {
    id: 2,
    label: 'Компоненты',
    labelText: 'Очень удобно использовать компоненты в своих проектах на vue js',
  },
  {
    id: 3,
    label: 'Router',
    labelText:
      'VUE очень хорошо сделали Router и плюсом его можно загрузить сразу при создании Vite',
  },
  {
    id: 4,
    label: 'Vuex',
    labelText:
      'Отличный сторе, где можно хранить данных. Если не знавится VueX, то используйте Pinia',
  },
  { id: 5, label: 'Compositions', labelText: 'Будь художником и изучай VUE' },
])

const CurrentStep = ref(1)

const NextStep = () => {
  if (CurrentStep.value < ItemsStep.length) {
    CurrentStep.value++
  }
}

const DownStep = () => {
  if (CurrentStep.value > 1) {
    CurrentStep.value--
  }
}

const currentText = computed(() => {
  return ItemsStep.find((i) => i.id === CurrentStep.value)
})

watch(CurrentStep, (newValue) => {
  localStorage.setItem('stepValue', `${CurrentStep.value}`)
})
</script>

<template>
  <div class="frist__section">
    <containerStyle class="container">
      <div class="main__inform">
        <h1>{{ title }}</h1>
        <div class="itemLabelText">
          <p>
            {{ currentText?.labelText }}
          </p>
        </div>
      </div>
      <div class="stepbystep">
        <ul class="ul__list">
          <li class="li__list" v-for="item in ItemsStep" :key="item.id">
            <span
              :class="[
                'id__count',
                {
                  id__count_active: CurrentStep === item.id,
                  id__count_done: CurrentStep > item.id,
                  id__count_default: CurrentStep < item.id,
                },
              ]"
              >{{ item.id }}</span
            >
            <p>{{ item.label }}</p>
          </li>
        </ul>
      </div>
      <div class="btn__div">
        <button v-if="CurrentStep > 1" class="btn down" @click="DownStep">Назад</button>
        <button v-if="CurrentStep < 5" class="btn next" @click="NextStep">Вперёд</button>
      </div>
    </containerStyle>
  </div>
</template>

<style scoped>
.frist__section {
  padding-top: 30px;
}

.container {
  padding: 20px;
  background: white;
  border-radius: 25px;
}

.main__inform {
  padding: 20px 40px;
}

.itemLabelText {
  margin-top: 20px;
}

.stepbystep {
  width: 100%;
}

.ul__list {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-around;
}

.li__list {
  width: max-content;
  display: flex;
  align-items: center;
  grid-gap: 5px;
}

.id__count {
  padding: 10px 15px;
  border-radius: 50px;
  color: white;
  font-weight: bold;
}

.id__count_active {
  background: green;
}

.id__count_done {
  background: darkgreen;
}

.id__count_default {
  background: rgb(201, 201, 201);
}

.btn__div {
  margin-top: 10px;
  width: max-content;
  margin-left: 40px;
  display: flex;
  grid-gap: 10px;
}

.btn {
  padding: 10px 20px;
  border-radius: 25px;
  border: none;
}

.down {
  background: #e9e9e9;
}

.next {
  background: rgb(98, 189, 98);
}
</style>
