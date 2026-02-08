<template>
  <div class="min-h-screen bg-gray-100 flex flex-col items-center py-12 px-4">
    <div class="max-w-md w-full bg-white rounded-3xl shadow-xl overflow-hidden">
      <div class="bg-green-500 p-6 text-white text-center">
        <h1 class="text-2xl font-bold tracking-tight">오늘 저녁 뭐 먹지? 🎰</h1>
        <p class="text-green-100 text-sm mt-1">결정 장애 해결사</p>
      </div>

      <div class="p-8 flex flex-col items-center">
        <div 
          class="w-full h-32 flex items-center justify-center rounded-2xl border-4 transition-all duration-75 mb-8"
          :class="[
            isSpinning ? 'border-green-400 bg-green-50 scale-105 shadow-inner' : 'border-gray-200 bg-gray-50'
          ]"
        >
          <span class="text-3xl font-black text-gray-800 text-center px-4 leading-tight">
            {{ currentMenu }}
          </span>
        </div>

        <div class="flex gap-3 w-full mb-8">
          <button 
            @click="startRoulette" 
            :disabled="isSpinning || menuList.length === 0"
            class="flex-1 bg-green-500 hover:bg-green-600 disabled:bg-gray-400 text-white font-bold py-4 rounded-xl transition-all shadow-lg active:transform active:scale-95"
          >
            {{ isSpinning ? '고르는 중...' : '룰렛 돌리기' }}
          </button>
          <button 
            @click="reset" 
            class="bg-gray-200 hover:bg-gray-300 text-gray-700 font-bold py-4 px-6 rounded-xl transition-all"
          >
            초기화
          </button>
        </div>

        <div class="w-full border-t pt-6">
          <h3 class="text-sm font-semibold text-gray-500 mb-3 uppercase tracking-wider">메뉴 추가하기</h3>
          <div class="flex gap-2">
            <input 
              v-model="newMenu" 
              @keyup.enter="addMenu"
              placeholder="예: 초밥, 떡볶이..." 
              class="flex-1 border border-gray-300 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-green-500"
            />
            <button @click="addMenu" class="bg-blue-500 text-white px-4 py-2 rounded-lg hover:bg-blue-600">추가</button>
          </div>
        </div>
      </div>

      <div class="bg-gray-50 p-6 border-t">
        <div class="flex flex-wrap gap-2">
          <span 
            v-for="(menu, idx) in menuList" :key="idx"
            class="bg-white border border-gray-200 px-3 py-1 rounded-full text-sm text-gray-600 flex items-center gap-1 shadow-sm"
          >
            {{ menu }}
            <button @click="removeMenu(idx)" class="text-gray-400 hover:text-red-500 ml-1">×</button>
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const menuList = ref(['삼겹살', '치킨', '피자', '마라탕', '파스타', '국밥', '신전떡볶이', '굶기', '햄버거']);
const currentMenu = ref('메뉴를 골라보세요');
const newMenu = ref('');
const isSpinning = ref(false);

const addMenu = () => {
  if (newMenu.value.trim()) {
    menuList.value.push(newMenu.value.trim());
    newMenu.value = '';
  }
};

const removeMenu = (index) => {
  menuList.value.splice(index, 1);
};

const startRoulette = () => {
  if (isSpinning.value || menuList.value.length === 0) return;
  
  isSpinning.value = true;
  let count = 0;
  const maxCount = 25; 

  const timer = setInterval(() => {
    const randomIndex = Math.floor(Math.random() * menuList.value.length);
    currentMenu.value = menuList.value[randomIndex];
    count++;

    if (count >= maxCount) {
      clearInterval(timer);
      isSpinning.value = false;
    }
  }, 80);
};

const reset = () => {
  currentMenu.value = '메뉴를 골라보세요';
};
</script>