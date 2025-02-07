<script setup>
import { computed } from '@vue/reactivity';
import { defineProps, watch, ref, reactive } from 'vue';

const props = defineProps({
    activeStep: Number,
});

const activeStep = ref(props.activeStep || 0);

watch(() => props.activeStep, (newVal) => {
    activeStep.value = newVal ?? 0;
});

const form = reactive({
    username: '',
    email: '',
});

const nextStep = () => {
    if (activeStep.value < 2) activeStep.value++;
};

const prevStep = () => {
    if (activeStep.value > 0) activeStep.value--;
};

const usernameError = computed(() => {
    const username = form.username.trim();
    if(activeStep.value !== 0) {
        return ''
    } else if(!username || !/^[a-zA-Z0-9_]+$/.test(username) || username.length < 4 || username.length > 30) {
        return 'Invalid Username.'
    } else {
        return ''
    }
})
const emailError = computed(() => {
    const email = form.emailError.trim();
    if(activeStep.value !== 1) {
        return ''
    } else if(!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email) || !email) {
        return 'Invalid email address.'
    } else {
        return ''
    }
})

const nextBtnDisabled = computed(() => {
    if (activeStep.value === 0) return !!usernameError.value;
    if (activeStep.value === 1) return !!emailError.value;
    return activeStep.value === 2;
})

const steps = ref([
    { Step: '1', title: 'Username' },
    { Step: '2', title: 'Email' },
    { Step: '3', title: 'Review' },
]);
</script>

<template>
  <div class="flex justify-center items-center">
    <template v-for="(step, index) in steps" :key="index">
        <div
            v-if="index !== 0 && index < steps.length"
            class="grow h-1 border-t-2 border-gray-400 transition-all duration-500 ease-in-out"
            :class="{'border-green-700': activeStep >= index }"
        >
        </div>
        <div class="flex justify-center flex-wrap cursor-default">
            <span
                class="p-6 border-2 w-32 text-center rounded-3xl border-gray-400 relative transition-all duration-500 ease-in-out !overflow-visible text-gray-400"
                :class="{
                'text-green-700': activeStep >= index,
                'border-green-700': activeStep >= index,
                }"
            >
                {{ step.title }}
            </span>
        </div>
    </template>
</div>
<div class="mt-8">
  <!-- Step 1: Username Input -->
    <div v-if="activeStep === 0" class="h-36">
      <label for="username" class="block mb-2 text-sm font-medium text-gray-700">Username:</label>
      <input
        id="username"
        v-model="form.username"
        type="text"
        placeholder="Ex. user_name"
        class="w-full px-3 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-green-700"
      />
      <p v-if="usernameError" class="text-red-600 text-sm mt-1">{{ usernameError }}</p>
    </div>
    
    <!-- Step 2: Email Input -->
    <div v-if="activeStep === 1" class="h-36">
        <label for="email" class="block mb-2 text-sm font-medium text-gray-700">Email:</label>
        <input
        id="email"
        v-model="form.email"
        type="email"
        class="w-full px-3 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-green-700"
        />
        <p v-if="emailError" class="text-red-600 text-sm mt-1">{{ emailError }}</p>
    </div>

    <!-- Step 3: Review -->
    <div v-if="activeStep === 2" class="flex flex-col h-36 gap-5">
      <h2 class="text-2xl font-bold">Review</h2>
      <p><strong>Username:</strong> {{ form.username }}</p>
      <p><strong>Email:</strong> {{ form.email }}</p>
    </div>
</div>

<!-- Wizard Controller -->
 <div class="mt-8 flex justify-between items-center">
    <button id="btn-next" :disabled="nextBtnDisabled" @click="nextStep()" 
    class="px-4 py-2 w-28 bg-green-700 text-white rounded-lg"
    :class="{'bg-gray-400' : nextBtnDisabled}"
    >Next</button>
    <button id="btn-prev" :disabled="activeStep === 0" @click="prevStep()" 
    class="px-4 py-2 w-28 border border-green-700 rounded-lg"
    :class="{'!border-gray-400 text-gray-400' : activeStep === 0}"
    >Previous</button>
 </div>
</template>