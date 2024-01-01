<script lang="ts" setup>
definePageMeta({
  middleware: ["guest"],
});

interface Credentials {
  username: string;
  password: string;
}

const { login } = useAuth();
const config    = useRuntimeConfig();
const router    = useRouter();

const credentials: Credentials = reactive({
  username: "test@example.com",
  password: "password",
});

const error = ref<string>("");

async function submit() {
    try {
        error.value = "";
        await login(credentials.username, credentials.password, true);
        router.push(config.public.homeUrl);
    } catch (err) {
        error.value = err as string;
    }
}
</script>

<template>
  <AuthCard>

    <template #logo>
      <NuxtLink to="/">
        <ApplicationLogo class="w-20 h-20 fill-current text-gray-500" />
      </NuxtLink>
    </template>

    <form @submit.prevent="submit">
      <small>{{ error }}</small>

      <!-- Email Address -->
      <div>
        <Label for="email">Email</Label>
        <Input
          id="username"
          type="email"
          class="block mt-1 w-full"
          v-model="credentials.username"
          required
          autoFocus
        />
      </div>

      <!-- Password -->
      <div class="mt-4">
        <Label for="password">Password</Label>
        <Input
          id="password"
          type="password"
          class="block mt-1 w-full"
          v-model="credentials.password"
          required
          autoComplete="current-password"
        />
      </div>

      <div class="flex items-center justify-end mt-4">
        <Button class="ml-3" :disabled="inProgress">Login</Button>
        <NuxtLink
          to="/password-reset"
          class="ml-2 underline text-sm text-gray-600 hover:text-gray-900"
        >
          Forgot password
        </NuxtLink>
      </div>
    </form>
  </AuthCard>
</template>

