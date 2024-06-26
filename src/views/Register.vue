<template>
    <!-- Landing page section -->
    <section class="landing hidden lg:block mb-10">
        <div class="p-8 flex flex-col items-center">
            <!-- Landing page welcome headings overlaid over mountain view -->
            <h1 class="text-4xl text-white font-bold mb-16 mt-48">
                Chronicle your White Mountain adventures
            </h1>
            <h3 class="text-white text-2xl mb-8">
                Log your hikes down to the finest details
            </h3>
            <a href="#registerSection" class="text-white mb-4 underline transition transform hover:scale-125 duration-300">
                Register below
            </a>
            <!-- Font Awesome down arrow icon -->
            <a href="#registerSection">
                <i class="fa-solid fa-arrow-down fa-beat fa-2xl mt-8 " style="color: #fafafa;"></i>
            </a>
        </div>
    </section>
    <!-- Account Registration Section -->
    <section id="registerSection" class="max-w-screen-lg mx-auto px-4 py-20 md:w-1/2 lg:w-1/3">
        <!-- Register Form -->
        <form @submit.prevent="register" class="p-8 flex flex-col w-full bg-lightStone rounded-md shadow-lg">
            <h1 class="text-3xl text-darkSky mb-4">Register</h1>
            <!-- Email input -->
            <div class="flex flex-col mb-2">
                <label for="email" class="mb-1 text-sm text-darkSky">Email</label>
                <input 
                    type="text" 
                    required 
                    class="p-2 focus:outline-none" 
                    id="email" 
                    v-model="email"
                >
            </div>
            <!-- Username input -->
            <div class="flex flex-col mb-2">
                <label for="username" class="mb-1 text-sm text-darkSky">Username</label>
                <input 
                    type="text" 
                    required 
                    class="p-2 focus:outline-none" 
                    id="username" 
                    v-model="username"
                >
            </div>
            <!-- Password Input -->
            <div class="flex flex-col mb-2">
                <label for="password" class="mb-1 text-sm text-darkSky">Password</label>
                <input 
                    type="password" 
                    required 
                    class="p-2 focus:outline-none" 
                    id="password" 
                    v-model="password"
                >
            </div>
            <!-- Confirm Password Input -->
            <div class="flex flex-col mb-2">
                <label for="confirmPassword" class="mb-1 text-sm text-darkSky">Confirm Password</label>
                <input 
                    type="password" 
                    required 
                    class="p-2 focus:outline-none" id="confirmPassword" 
                    v-model="confirmPassword"
                >
            </div>
            <!-- Registration Submit Button -->
            <button type="submit" class="mt-6 py-2 px-6 rounded-sm self-start text-sm text-lightStone bg-darkSky duration-200 border-solid border-2 border-transparent hover:bg-lightStone hover:text-darkSky hover:border-darkSky">
                Register
            </button>
            <router-link 
                class="text-sm mt-6 text-center" 
                :to="{ name: 'Login' }"
            >
                Already have an account? <span class="text-sky-400 hover:text-lightSky">Login</span>
            </router-link>
        </form>
    </section>
</template>

<script setup>
    // Imports:
    import { ref } from 'vue';
    import { supabase } from '../supabase/supabaseClient.js';
    import { useRouter } from 'vue-router';
    import { useToast } from 'vue-toastification';

    // Data:
    const router = useRouter();
    const email = ref(null);
    const username = ref(null);
    const password = ref(null);
    const confirmPassword = ref(null);
    const toast = useToast();

    // Methods:

    // Register function
    const register = async () => {
        // If the password matches the confirmed password
        if (password.value === confirmPassword.value) {
            try {
                // Try to sign up the user
                const { error } = await supabase.auth.signUp({
                    email: email.value,
                    password: password.value,
                    options: {
                        data: {
                            username: username.value,
                        },
                    },
                });
                // If supabase presents an error, throw it:
                if (error) throw error;
                // Alert user to check their email to confirm
                toast.info('Check your email to confirm your account');
                // If everything checks out, push the user to the Login page
                router.push({ name: "Login" });
            }
            catch (error) {
                // Alert the user of the Supabase error
                toast.error(`Error: ${error.message}`);
            }
            return;
        }
        // If the passwords do not match:
        else {
            toast.warning('Passwords do not match');
        }
    };
</script>

<style>
    .landing {
        background-image: url('@/assets/images/backgrounds/twilight.jpg');
        background-size: cover;
        background-position: center;
        padding-top: 350px;
        padding-bottom: 350px;
        padding-left: 500px;
        padding-right: 500px;
    }
</style>