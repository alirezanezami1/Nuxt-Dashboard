<script setup >
import { Tabs, TabsContent, TabsList, TabsTrigger } from '@/components/ui/tabs';
const loading = ref(false)
let data = ref([])
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

let currentCategory = ref("today")

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
  generateRandomValue(24)
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
        <TabsContent class="w-[100%]" v-for="item , index in list" :key="index" :value="item.title">
          <Chart v-if="data.length > 0" :currentCategory="currentCategory" :data="data"></Chart>
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