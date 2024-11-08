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
  'today' : [],
  'week' : [],
  'month' : [],
  'year' : []
})
let currentCategories = ref(['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep','Oct', 'Nov', 'Dec'])

const options = computed(() => (
  {
    chart: {
        type: 'line',
        animation : {
          enabled : false
        }
    },
    legend : {
      enabled : false
    },
    title: {
        text: ''
    },
    xAxis: {
      gridLineColor: 'transparent',
        categories: currentCategories
    },
    yAxis: {
       gridLineColor: 'transparent',
        title: {
            text: ''
        }
    },
    plotOptions: {
        line: {
          marker : {
            enabled : false
          },
            dataLabels: {
                enabled: true
            },
            enableMouseTracking: true
        }
    },
    series: [{
        name: 'Line1',
        lineWidth : '4px',
        color : {
          linearGradient : {},
          stops : [
            [0, 'yellow'],
            [.5 , 'green'],
            [.88 , 'green'],
            [1 , 'orange']
          ]
        },
        data: [
            16.0, 18.2, 23.1, 25.2, 27.1, 31.7, 34.11, 36.68, 39.2, 45.2,
            38.0, 27.9
        ]
    }, {
        name: 'Line2',
        lineWidth : '4px',
        color : {
          linearGradient : {},
          stops : [
            [0, 'red'],
            [.5 , 'green'],
            [1 , 'yellow']
          ]
        },
        data: [
            -2.9, -3.6, -0.6, 4.8, 10.2, 14.5, 17.1, 16.5, 12.0, 6.5,
            2.0, -0.9
        ]
    }]
}
))

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
      
      <Tabs default-value="Today" >
        <TabsList class="w-[400px]">
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