<script setup>
import { ref } from 'vue';

const email = ref('');
const name = ref('');

const emailError = ref('');
const nameError = ref('');
const submitError = ref('');
const submitSuccess = ref(false);
const loading = ref(false);

const submit = () => {
  loading.value = true;
  // reset errors
  emailError.value = '';
  nameError.value = '';

  // validate input
  if (email.value === '') {
    emailError.value = 'Email is required';
  } else if (!/\S+@\S+\.\S+/.test(email.value)) {
    emailError.value = 'Email is invalid';
  }

  if (name.value === '') {
    nameError.value = 'Name is required';
  }

  // submit form to https://formspree.io/f/xvgpypkp
  if (emailError.value === '' && nameError.value === '') {
    console.log("submitting form...");
    const form = new FormData();
    form.append('Email', email.value);
    form.append('Name', name.value);

    fetch('https://script.google.com/macros/s/AKfycbxfS_e4kpuWSbRYqomeVLQE4EOzdT2hVeOq8krEZXQoj5G7d36rHJYBhaEwhd0WjoZQuQ/exec', {
      method: 'POST',
      // mode: 'no-cors',
      body: form
    }).then(response => {
      console.log(response);
      if (response.ok) {
        submitSuccess.value = true;
        email.value = '';
        name.value = '';
      } else {
        submitError.value = 'Error submitting form';
      }

      loading.value = false;
    }).catch(error => {
      loading.value = false;
      submitError.value = 'Error submitting form';
    });
  }
};

</script>

<template>
  <!-- basic waitlist sign up confirmation banner -->
  <Transition name="slide-down">
    <div v-if="submitSuccess" class="w-full bg-emerald-500 text-white text-center">
      <p class="p-3">Thank you for joining the waitlist!</p>
    </div>
  </Transition>

  <div
    class="h-screen w-full bg-gradient-to-br from-emerald-500 to-teal-600 flex flex-col md:flex-row items-center font-sans">
    <div class="w-full md:w-1/2 bg-gray-200 bg-opacity-80 m-12 rounded-lg shadow-lg p-8 md:order-2">
      <div class="text-center">
        <div class="flex flex-row justify-center items-center space-x-2">
          <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="42"
            zoomAndPan="magnify" viewBox="0 0 375 374.999991" height="42" preserveAspectRatio="xMidYMid meet"
            version="1.0">
            <!-- Use currentColor so it inherits the color from the parent -->
            <path class="fill-green-600"
              d="M 46.878906 -0.015625 L 328.136719 -0.015625 C 353.921875 -0.015625 375.015625 21.078125 375.015625 46.863281 L 375.015625 328.121094 C 375.015625 353.90625 353.921875 375 328.136719 375 L 46.878906 375 C 21.09375 375 0 353.90625 0 328.121094 L 0 46.863281 C 0 21.078125 21.09375 -0.015625 46.878906 -0.015625 "
              fill-opacity="1" fill-rule="evenodd" />
            <path fill="#ffffff"
              d="M 187.507812 58.542969 L 46.878906 128.707031 L 46.878906 164.019531 L 70.308594 164.019531 L 70.308594 140.574219 L 304.695312 140.574219 L 304.695312 164.019531 L 328.136719 164.019531 L 328.136719 128.707031 Z M 187.507812 237.917969 L 328.136719 167.742188 L 328.136719 316.441406 L 46.878906 316.441406 L 46.878906 167.742188 L 187.507812 237.917969 "
              fill-opacity="1" fill-rule="evenodd" />
          </svg>
          <h1 class="text-4xl font-extrabold text-gray-800">Newsletta</h1>
        </div>
        <p class="mt-2 text-lg text-gray-600">
          The easy way to manage a newsletter
        </p>
      </div>
      <ul class="mt-8 space-y-4 text-gray-700">
        <li class="flex items-center">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 mr-2 text-[#000000]" fill="none" viewBox="0 0 24 24"
            stroke="currentColor" style="min-width: 1.5rem; min-height: 1.5rem;">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
              d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" />
          </svg>
          Create custom newsletter sign up pages for followers
        </li>
        <li class="flex items-center">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 mr-2 text-[#000000]" fill="none" viewBox="0 0 24 24"
            stroke="currentColor" style="min-width: 1.5rem; min-height: 1.5rem;">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
              d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" />
          </svg>
          Embed the newsletter sign up component into other projects
        </li>
        <li class="flex items-center">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 mr-2 text-[#000000]" fill="none" viewBox="0 0 24 24"
            stroke="currentColor" style="min-width: 1.5rem; min-height: 1.5rem;">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
              d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" />
          </svg>
          View analytics for the newsletter
        </li>
      </ul>
      <!-- Link form to submit function -->
      <form class="mt-8" @submit.prevent="submit">
        <div class="mb-4">
          <label for="name" class="block text-gray-700 font-medium">Name</label>
          <input type="text" name="name" placeholder="Your name" v-model="name"
            class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:border-indigo-500"
            required />
          <p class="text-red-500">{{ nameError }}</p>
        </div>
        <div class="mb-6">
          <label for="email" class="block text-gray-700 font-medium">Email</label>
          <input type="email" name="email" placeholder="Your email" v-model="email"
            class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:border-indigo-500"
            required />
          <p class="text-red-500">{{ emailError }}</p>
        </div>

        <!-- submit button -->
        <button type="submit" v-show="!loading" :disabled="loading"
          class="w-full px-4 py-2 font-semibold text-white bg-indigo-600 rounded-md hover:bg-indigo-500 transition duration-300">
          Join Waitlist
        </button>
        <p class="text-red-500">{{ submitError }}</p>

        <!-- Loading Spinner -->
        <div v-show="loading" class="flex justify-center">
          <svg aria-hidden="true" class="w-8 h-8 text-gray-200 animate-spin dark:text-gray-600 fill-blue-600"
            viewBox="0 0 100 101" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path
              d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z"
              fill="currentColor" />
            <path
              d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z"
              fill="currentFill" />
          </svg>
        </div>

        <!-- <p class="text-green-600" v-if="submitSuccess">Thank you for joining the waitlist!</p> -->
        <p class="mt-6 text-center text-sm text-gray-500">
          Made with <span class="text-red-500">love</span> by <a href="https://x.com/robert_watkin_"
            class="text-indigo-600 underline hover:text-indigo-800">Robert Watkin</a>
        </p>
      </form>
    </div>
    <div class="w-full md:w-1/2 bg-cover bg-center h-full md:order-1"
      style="background-image: url('https://images.unsplash.com/photo-1519389950473-47ba0277781c?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1950&q=80')">
    </div>
  </div>
</template>

<style>
.slide-down-enter-active,
.slide-down-leave-active {
  transition: all 0.5s ease;
}

.slide-down-enter-from,
.slide-down-leave-to {
  transform: translateY(-100%);
}
</style>