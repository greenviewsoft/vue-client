
<script setup>
const form = reactive({
  email: null,
  password: null,
});

const errors = ref([]);
const HandleSubmit = async () => {
  try {
    const { data } = await $fetch('http://localhost:8000/api/login', {
      method: 'POST',
      body: { ...form },
    });
  } catch (error) {
    if (error.data && error.data.errors) {
      errors.value = error.data.errors;
    } else {
      // Handle the error in case there is no 'data' property in the error response.
      console.error('Error:', error);
    }
  }
};

</script>

<template> 
    <div class="main-h-screen flex items-center">
        <div class="w-full"> 
            <div class="card bg-white p-8 rounded-lg shadow-xl md:w-3/4 mx-auto lg:w-1/3">

<h3 class="text-center text-2xl font-semibold">User Login</h3>

      {{ form }}
<form @submit.prevent="HandleSubmit">
  <div class="mb-6">
   <FormLabel>Email </FormLabel>
   <FormInputText   id="email" placeholder="Email" type="email"    v-model="form.email" />
   <span v-if="errors.email" class="text-red-500">{{ errors.email[0] }}</span>
  </div>
  <div class="mb-6">
    <FormLabel>Password </FormLabel>
    <FormInputText  id="password" placeholder="Password" type="password"    v-model="form.password" />
    <span v-if="errors.password" class="text-red-500">{{ errors.password[0] }}</span>
  </div>
  <div class="flex items-start mb-6">
    <div class="flex items-center h-5">
      <input id="remember" type="checkbox" value="" class="w-4 h-4 border border-gray-300 rounded bg-gray-50 focus:ring-3 focus:ring-blue-300 dark:bg-gray-700 dark:border-gray-600 dark:focus:ring-blue-600 dark:ring-offset-gray-800 dark:focus:ring-offset-gray-800">
    </div>
    <FormLabel class="ml-2" for="remember">Remember me </FormLabel>
  </div>

<ButtonPrimary>Login</ButtonPrimary>

</form>

<SocialLogin/>

    </div>
</div>
</div>

    </template>