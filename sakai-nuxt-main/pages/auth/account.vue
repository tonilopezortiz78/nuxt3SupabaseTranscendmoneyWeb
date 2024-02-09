<script setup>
    const user= useSupabaseUser();
    const client = useSupabaseClient();
    const router= useRouter();
    const successMsg=ref(null);
    const errorMsg=ref(null)


    async function signOut() {
        try{
            const { error } = await client.auth.signOut()
            if (error) throw error;
            router.push('/login');
            successMsg="SignOUt";
        } catch (error){
            errorMsg=error.value;
        }
    }
</script>
<template>
    <h1> Welcome: {{ user.email }}</h1>
    <Button @click="signOut"> Sign Out</Button>
</template>
