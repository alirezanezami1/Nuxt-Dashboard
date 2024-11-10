<script setup >
import { Tabs, TabsContent, TabsList, TabsTrigger } from '@/components/ui/tabs';
const loading = ref(false)
const list = [
  {
    title : "Today",
    component: resolveComponent("TabsToday")
  },
  {
     title : "Week",
     component: resolveComponent("TabsWeek")
  },
  {
     title : "Month",
     component: resolveComponent("TabsMonth")
  },
  {
     title : "Year",
     component: resolveComponent("TabsYear")
  }
]



let categories = ref({
  today: [
    "00:00",
    "01:00",
    "02:00",
    "03:00",
    "04:00",
    "05:00",
    "06:00",
    "07:00",
    "08:00",
    "09:00",
    "10:00",
    "11:00",
    "12:00",
    "13:00",
    "14:00",
    "15:00",
    "16:00",
    "17:00",
    "18:00",
    "19:00",
    "20:00",
    "21:00",
    "22:00",
    "23:00",
  ],
  week: [
    'Sunday',
    'Monday',
    'Tuesday',
    'Wednesday',
    'Thursday',
    'Friday',
    'Saturday',
  ],
  year: [
    'Jan',
    'Feb',
    'Mar',
    'Api',
    'May',
    'Jun',
    'Jul',
    'Aug',
    'Sep',
    'Oct',
    'Nov',
    'Dec'
  ]
})

let data = ref([])

let currentCategory = ref("today")

const options = computed(() => ({
    chart: {
        type: 'line',
        animation : {
          enabled : false
        }
    },
    title: {
        text: ''
    },
    xAxis: {
      gridLineColor: 'transparent',
        categories: categories.value[currentCategory.value]
    },
    yAxis: {
       gridLineColor: 'transparent',
        title: {
            text: ''
        }
    },
    legend : {
      enabled : false
    },
    plotOptions: {
        line: {
          marker : {
            enabled : false
          },
            dataLabels: {
                enabled: false
            },
            enableMouseTracking: false
        }
    },
    series: [ {
        name: 'line',
        lineWidth : '4px',
        color : {
          linearGradient : {},
          stops : [
            [0, 'red'],
            [.5 , 'green'],
            [1 , 'yellow']
          ]
        },
        data: data.value
    }]
}
))


////////////////////////////////////////
// function to generator current month

function generateMonth() {
  let currentDate = new Date();
  let currentMonth = currentDate.getMonth() + 1;

  let currentYear = currentDate.getFullYear()

  function generateMonthDates() {
    let monthDates = [];
    let daysInMonth = new Date(currentYear , currentMonth, 0).getDate();


    for(let i = 1; i <= daysInMonth ; i++) {
      let dayString = ("0" + i).slice(-2);
      let monthString = ("0" + currentMonth).slice(-2);

      monthDates.push(monthString + "/" + dayString);
    }

    return monthDates

  }

  let month = generateMonthDates()
  categories.value = ({ ...categories.value, month})
  return month;

}



function generateRandomValue(number = 7) {
  let values = []
  for (let i = 0 ; i < number + 1; i++) {
    values.push(Math.floor(Math.random() * 100))
  }
  data.value = values
  return values
}



const setCategory = (e) => {
  let v = e.target.innerText.toLowerCase()
  currentCategory.value = v
  switch (v) {
    case 'today': 
      generateRandomValue(24);
      break;
    case 'week': 
      generateRandomValue(7);
      break;
    case 'month': 
      generateRandomValue(31);
      break;
    case 'year': 
      generateRandomValue(12);
      break;
  }
}



onMounted(() => {
  generateMonth()
  generateRandomValue(7)
})

</script>

<template>
  <div class="grid w-full gap-4">
    <header class="flex items-start justify-between">
      <div class="grow">
        <p>Hi, welcome back...</p>
        <h1>Dashboard</h1>
      </div>
      <div class="w-[120px] h-[36px] bg-neutral-200"></div>
    </header>
    <main class="grid gap-2">
      
      <Tabs default-value="Today" @click="setCategory">
        <TabsList class="max-w-[400px]">
          <TabsTrigger v-for="item , index in list" :key="index" :value="item.title">
            {{ item.title }}
          </TabsTrigger>
        </TabsList>
        <TabsContent v-for="item , index in list" :key="index" :value="item.title">
          <highchart :options="options" />
        </TabsContent>
      </Tabs>

      <!-- <div class="flex items-center gap-4">
        <div v-for="(item,index) in 3" :key="index" class="w-[120px] h-[36px] bg-neutral-200"></div>
      </div>
      <section>
        <div class="w-full h-[360px] bg-neutral-200"></div>
      </section> -->
    </main>
    <footer>
      <div class="flex items-center gap-4">
        <div v-for="(item,index) in 3" :key="index" class="w-full h-[260px] bg-neutral-200"></div>
      </div>
    </footer>
  </div>
</template>