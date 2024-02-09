<script setup>
import { useLayout } from '@/layouts/composables/layout';
import { ref, computed } from 'vue';
import AppConfig from '@/layouts/AppConfig.vue';
const { layoutConfig } = useLayout();
const email = ref('');
const password = ref('');
const checked = ref(false);
const client = useSupabaseClient();
const successMsg=ref(null)
const errorMsg=ref(null)
const user = useSupabaseUser()

const logoUrl = computed(() => {
    return `/layout/images/${layoutConfig.darkTheme.value ? 'tmlogo' : 'tmlogo'}.svg`;
});


async function magicLink() {
    try{
        const { data, error } = await client.auth.signInWithOtp({
            email: email.value,
            options: {
            // set this to false if you do not want the user to be automatically signed up
            emailRedirectTo: 'http://localhost:3000/confirm',
            },
        })
        if (error) throw error;
        successMsg.value="Check your email and click the magic link to login"
        errorMsg.value=null
    } catch (error){
        errorMsg.value=error.message;
        successMsg.value=null;
    }
}

watch(user, () => {
if (user.value) {
    // Redirect to protected page
    console.log('push navigation',user.value)
    return navigateTo('/')
}
}, { immediate: true })

definePageMeta({
    layout: false
});
</script>

<template>
    <div class="surface-ground flex align-items-center justify-content-center min-h-screen min-w-screen overflow-hidden">
        <div class="flex flex-column align-items-center justify-content-center">
            <!--
            <img :src="logoUrl" alt="Sakai logo" class="mb-5 w-6rem flex-shrink-0" />
            -->
            <div style="border-radius: 56px; padding: 0.3rem; background: linear-gradient(180deg, var(--primary-color) 10%, rgba(33, 150, 243, 0) 30%)">
                <div class="w-full surface-card py-8 px-5 sm:px-8" style="border-radius: 53px">
                    <div class="text-center mb-5">
                        <!-- <img :src="logoUrl" alt="transcend money logo" class="mb-5 w-6rem flex-shrink-0" />
                        <img src="/demo/images/login/avatar.png" alt="Image" height="50" class="mb-3" /> -->
                        <div class="text-900 text-3xl font-medium mb-3">Welcome back to</div> 
                        <div class="text-900 text-3xl font-medium mb-3">Transcend Money</div> 
                        <Fieldset legend="help (magic link)" :toggleable="true">
                            <p>
                                1-Insert your email, click Sign in
                            </p>
                            <p>
                                2-Wait confirmation, about 3 seconds
                            </p>
                            <p>
                                3-check your email and click the link
                            </p>
                                
                        </Fieldset>
                    </div>

                        <label for="email1" class="block text-900 text-xl font-medium mb-2">Email</label>
                    <div class="flex flex-column align-items-center justify-content-center">
                        <InputText  id="email1" v-model="email" type="email" placeholder="Email address" class="w-full md:w-30rem mb-5" style="padding: 1rem" />
                        <!--
                        <label for="password1" class="block text-900 font-medium text-xl mb-2">Password</label>
                        <Password id="password1" v-model="password" placeholder="Password" :toggleMask="true" class="w-full mb-3" inputClass="w-full" :inputStyle="{ padding: '1rem' }"></Password>

                        <div class="flex align-items-center justify-content-between mb-5 gap-5">
                            <div class="flex align-items-center">
                                <Checkbox id="rememberme1" v-model="checked" binary class="mr-2"></Checkbox>
                                <label for="rememberme1">Remember me</label>
                            </div>
                            <a class="font-medium no-underline ml-2 text-right cursor-pointer" style="color: var(--primary-color)">Forgot password?</a>
                        </div>
                    -->
                        <p v-if="errorMsg" style="font-weight:bold; color:red "> {{ errorMsg }}</p>
                        <p v-if="successMsg" style="font-weight:bold; color:green">  {{ successMsg}}</p>
                        <Button @click="magicLink" label="Sign In" class="w-full p-3 text-xl"></Button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <AppConfig simple />
</template>

<style scoped>
.pi-eye {
    transform: scale(1.6);
    margin-right: 1rem;
}
.pi-eye-slash {
    transform: scale(1.6);
    margin-right: 1rem;
}
</style>
