<script setup lang="ts">
import { ref } from 'vue';


interface DataCourses {
  [key: string]: DataCoursesInfo
}
interface DataCoursesInfo {
  lesson_count_month: number,
  month: number,
  launch_count_month: number,
  video_count: number,
  video_size: number,
  price: number,
  month_amount: MonthDataCoursesInfoAmount[]
}

interface DataCoursesInfoAmount {
  video_count: number,
  video_size: number,
  price: number,
}
interface MonthDataCoursesInfoAmount {
  month: number,
  amount: DataCoursesInfoAmount
}
const price_course = ref<number>(150)
const price = ref<number>(0.08)
const video_size_mb = ref<number>(250)
const video_size = 1024
const month_count = 36
const delete_month = ref<number[]>([3, 6, 9, 12])
const addMounth = ref<string>('')
const total_amount = ref<MonthDataCoursesInfoAmount[]>([])
const courses = ref<DataCourses>({
  'ВП РУС': {
    month_amount: [], lesson_count_month: 8, month: 10, launch_count_month: 4, video_count: 0, video_size: 0, price: 0,
  },
  'ВП УЗБ': {
    month_amount: [], lesson_count_month: 8, month: 8, launch_count_month: 3, video_count: 0, video_size: 0, price: 0,
  },
  'ДИЗ РУС': {
    month_amount: [], lesson_count_month: 8, month: 6, launch_count_month: 3, video_count: 0, video_size: 0, price: 0,
  },
  'ДИЗ УЗБ': {
    month_amount: [], lesson_count_month: 8, month: 6, launch_count_month: 2, video_count: 0, video_size: 0, price: 0,
  },
  'СММ РУС': {
    month_amount: [], lesson_count_month: 8, month: 4, launch_count_month: 3, video_count: 0, video_size: 0, price: 0,
  },
  'СММ УЗБ': {
    month_amount: [], lesson_count_month: 8, month: 4, launch_count_month: 3, video_count: 0, video_size: 0, price: 0,
  },
  'BLENDER РУС': {
    month_amount: [], lesson_count_month: 8, month: 4, launch_count_month: 2, video_count: 0, video_size: 0, price: 0,
  },
  'BLENDER УЗБ': {
    month_amount: [], lesson_count_month: 8, month: 4, launch_count_month: 2, video_count: 0, video_size: 0, price: 0,
  },
  'ИМ РУС': {
    month_amount: [], lesson_count_month: 8, month: 4, launch_count_month: 2, video_count: 0, video_size: 0, price: 0,
  },
  'ИМ УЗБ': {
    month_amount: [], lesson_count_month: 8, month: 4, launch_count_month: 0, video_count: 0, video_size: 0, price: 0,
  },
  '3Д РУС': {
    month_amount: [], lesson_count_month: 8, month: 5, launch_count_month: 3, video_count: 0, video_size: 0, price: 0
  },
  '3Д УЗБ': {
    month_amount: [], lesson_count_month: 8, month: 5, launch_count_month: 4, video_count: 0, video_size: 0, price: 0
  },
  'PY РУС': {
    month_amount: [], lesson_count_month: 8, month: 8, launch_count_month: 4, video_count: 0, video_size: 0, price: 0
  },
  'PY УЗБ': {
    month_amount: [], lesson_count_month: 8, month: 8, launch_count_month: 2, video_count: 0, video_size: 0, price: 0
  },
  'ВМ РУС': {
    month_amount: [], lesson_count_month: 8, month: 5, launch_count_month: 3, video_count: 0, video_size: 0, price: 0
  },
  'ВМ УЗБ': {
    month_amount: [], lesson_count_month: 8, month: 5, launch_count_month: 1, video_count: 0, video_size: 0, price: 0
  },
  'MS РУС': {
    month_amount: [], lesson_count_month: 8, month: 2, launch_count_month: 3, video_count: 0, video_size: 0, price: 0
  },
  'MS УЗБ': {
    month_amount: [], lesson_count_month: 8, month: 2, launch_count_month: 3, video_count: 0, video_size: 0, price: 0
  },
  'МОБИЛОГРАФИЯ': {
    month_amount: [], lesson_count_month: 12, month: 2, launch_count_month: 2, video_count: 0, video_size: 0, price: 0
  },
  'ФОТО': {
    month_amount: [], lesson_count_month: 12, month: 2, launch_count_month: 2, video_count: 0, video_size: 0, price: 0
  },
  'КС РУС': {
    month_amount: [], lesson_count_month: 8, month: 2, launch_count_month: 2, video_count: 0, video_size: 0, price: 0
  },
  'КС УЗБ': {
    month_amount: [], lesson_count_month: 8, month: 2, launch_count_month: 2, video_count: 0, video_size: 0, price: 0
  },
  'РЕВИТ РУС': {
    month_amount: [], lesson_count_month: 8, month: 2, launch_count_month: 0, video_count: 0, video_size: 0, price: 0
  },
  'РЕВИТ УЗБ': {
    month_amount: [], lesson_count_month: 8, month: 2, launch_count_month: 1, video_count: 0, video_size: 0, price: 0
  },
})

function add_month() {
  if (+addMounth.value) {
    const find = delete_month.value.find(a => a == +addMounth.value)
    if (!find) {
      delete_month.value.push(+addMounth.value)
      addMounth.value = ''
    }
  }
}
function delMounth(val: number) {
  delete_month.value = delete_month.value.filter(a => a != val)
}

function totalAmount() {
  total_amount.value = []
  for (const key in courses.value) {
    const course = courses.value[key]
    course.month_amount = []
  }
  for (const key in courses.value) {
    const course = courses.value[key]
    for (const monthKey in delete_month.value) {
      const monthItem = delete_month.value[monthKey]
      for (let i = 1; i <= month_count; i++) {
        const group = i <= course.month ? i : course.month
        const find = course.month_amount.find(a => a.month == monthItem)
        if (!find) {
          const amountMonth: MonthDataCoursesInfoAmount = {
            month: monthItem,
            amount: {
              price: 0,
              video_count: 0,
              video_size: 0
            }
          }
          course.month_amount.push(amountMonth)
        }
        const findData = course.month_amount.find(a => a.month == monthItem)
        if (findData) {
          findData.amount.video_count += group * course.launch_count_month * course.lesson_count_month
          if ((course.month + monthItem) < i) {
            findData.amount.video_count -= group * course.launch_count_month * course.lesson_count_month
          }
        }
      }

      const findData = course.month_amount.find(a => a.month == monthItem)
      if (findData) {
        findData.amount.video_size = findData.amount.video_count * (video_size_mb.value / video_size)
        findData.amount.price = findData.amount.video_size * price.value
        const findTotal = total_amount.value.find(a => a.month == monthItem)
        if (!findTotal) {
          const amountMonth: MonthDataCoursesInfoAmount = {
            month: monthItem,
            amount: {
              price: 0,
              video_count: 0,
              video_size: 0
            }
          }
          total_amount.value.push(amountMonth)
        }

        const findTotalData = total_amount.value.find(a => a.month == monthItem)
        if (findTotalData) {
          findTotalData.amount.video_count += findData.amount.video_count
          findTotalData.amount.video_size += findData.amount.video_size
          findTotalData.amount.price += findData.amount.price
        }
      }
      setTimeout(() => {
        const html = document.querySelector<HTMLElement>('html')
        if (html) {
          html.scrollTo({ top: 0, behavior: 'smooth' })
        }
      }, 200)
    }
  }
}

function reset() {
  const data = confirm('Вы уверены что хотите сбросить данные')
  if (data) {
    total_amount.value = []
    for (const key in courses.value) {
      const course = courses.value[key]
      course.month_amount = []
    }
  }
}
</script>

<template>
  <div class="p20 fc10">
    <div>Курс рубля</div>
    <li class="listmonth-item">
      <input v-model="price_course" /> сум
    </li>
    <div>Цена за 1гб в день</div>
    <li class="listmonth-item">
      <input v-model="price" /> рублей
    </li>
    <div>Средний размер видео</div>
    <li class="listmonth-item">
      <input v-model="video_size_mb" /> мб
    </li>
    <div>Месяца подсчёта</div>
    <ul class="listmonth" v-if="delete_month.length">
      <li class="listmonth-item" v-for="month in delete_month.sort((a, b) => a - b)">
        {{ month }} мес.
        <button @click="() => delMounth(month)">Удалить</button>
      </li>
    </ul>
    <li class="listmonth-item">
      <input v-model="addMounth" placeholder="Добавить месяц" @keyup.enter="add_month" />
    </li>
    <button style="display: flex; padding: 10px;width: 100%; justify-content: center;" @click="totalAmount">Подсчёт</button>
    <button v-if="total_amount.length" style="display: flex; padding: 10px;width: 100%; justify-content: center;" @click="reset">Сброс</button>
    <div v-if="total_amount.length">Общие данные</div>
    <div class="courses_item-tab" v-for="month in total_amount">
      <div class="courses_item-data">
        Месяц хранения: <b>{{ month.month }}</b>
      </div>
      <div class="courses_item-data">
        <b>{{ month.amount.price }}</b> руб за день <br> <b>{{ (month.amount.price * price_course).toLocaleString() }}</b> сум за день <br> <b>{{ (month.amount.price * price_course * 30).toLocaleString() }}</b> сум за месяц
      </div>
      <div class="courses_item-data">
        <b>{{ month.amount.video_count.toLocaleString() }}</b> кол-во видео
      </div>
      <div class="courses_item-data">
        <b>{{ month.amount.video_size.toLocaleString() }}</b> ГБ
      </div>
    </div>
    <div>Данные по курсам</div>
    <div class="courses">
      <div class="courses_item" v-for="course, name in courses">
        <div class="courses_item-data">{{ name }}</div>
        <div class="courses_item-data">Групп в месяц: <input v-model="course.launch_count_month"></div>
        <div class="courses_item-data">Блоков: {{ course.month }}</div>
        <div class="courses_item-data">Уроков в блоке: {{ course.lesson_count_month }}</div>
        <div class="courses_item-data-list" v-if="course.month_amount.length > 0">
          <div class="courses_item-tab" v-for="month in course.month_amount">
            <div class="courses_item-data">
              Месяц хранения: {{ month.month }}
            </div>
            <div class="courses_item-data">
              <b>{{ month.amount.price }}</b> руб за день <br> <b>{{ (month.amount.price * price_course).toLocaleString() }}</b> сум за день <br> <b>{{ (month.amount.price * price_course * 30).toLocaleString() }}</b> сум за месяц
            </div>
            <div class="courses_item-data">
              <b>{{ month.amount.video_count }}</b> кол-во видео
            </div>
            <div class="courses_item-data">
              Размер <b>{{ month.amount.video_size.toLocaleString() }}</b> ГБ
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <button style="display: flex; padding: 10px;width: 100%; justify-content: center;" @click="totalAmount">Подсчёт</button><br>
  <button v-if="total_amount.length" style="display: flex; padding: 10px;width: 100%; justify-content: center;" @click="reset">Сброс</button>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Open+Sans:ital,wght@0,300..800;1,300..800&display=swap');

* {
  margin: 0;
  padding: 0;
  font-family: "Open Sans", sans-serif;
  font-size: 14px;
}

body {
  font-family: "Open Sans", sans-serif;
  font-size: 14px;
}

.p20 {
  padding: 20px;
}

.fc10 {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.courses {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.courses_item {
  display: flex;
  flex-wrap: wrap;
}

.courses_item-data {
  flex-grow: 1;
  padding: 5px;
  background: #ececec;
  font-size: 14px;
}

.courses_item-data-list {
  width: 100%;
}

.courses_item-tab {
  display: flex;
  flex-wrap: wrap;
  flex-grow: 1;
  padding: 5px;
  background: #ececec;
  font-size: 14px;
}

.listmonth {
  list-style: none;
  display: flex;
  flex-wrap: wrap;
  gap: 5px;
}

.listmonth-item {
  flex-grow: 1;
  padding: 10px;
  background: #ececec;
  font-size: 14px;
  border-radius: 6px;
  list-style: none;
  min-width: max-content;
}
</style>