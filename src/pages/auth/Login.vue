<template>
   <q-page class="auth row items-center justify-center q-pa-sm">
      <q-card class="auth__card q-px-xl q-py-lg">
         <q-card-section>
            <h1 class="text-h4 q-mt-none q-mb-xl">Sign in</h1>
            <q-form @submit.prevent="onSubmit" class="q-gutter-md column">
               <q-input
                  autocorrect="off"
                  autocapitalize="off"
                  autocomplete="off"
                  spellcheck="false"
                  v-model="email"
                  outlined
                  label="email"
                  :rules="[
                     (val) => (val && val.length > 0) || 'email is required',
                  ]"
               />

               <q-input
                  autocorrect="off"
                  autocapitalize="off"
                  autocomplete="off"
                  spellcheck="false"
                  v-model="password"
                  outlined
                  label="password"
                  :type="isPwd ? 'password' : 'text'"
                  :rules="[
                     (val) => (val && val.length > 0) || 'password is required',
                  ]"
               >
                  <template v-slot:append>
                     <q-icon
                        :name="isPwd ? 'visibility_off' : 'visibility'"
                        class="cursor-pointer"
                        @click="isPwd = !isPwd"
                     />
                  </template>
               </q-input>

               <div class="q-mt-lg">
                  <q-btn
                     class="block full-width"
                     label="Login"
                     type="submit"
                     color="primary"
                  />
                  <div class="column items-center q-mt-md">
                     <label>
                        Don't have an account?
                        <router-link
                           :to="{ path: 'signup' }"
                           class="text-weight-bold text-primary"
                        >
                           Signup
                        </router-link>
                     </label>
                     <a class="text-weight-bold text-primary q-mt-sm">
                        Forget password?
                     </a>
                  </div>
               </div>
            </q-form>
         </q-card-section>
      </q-card>
   </q-page>
</template>

<script>
import { defineComponent, ref } from "vue";
import { useStore } from "vuex";
import { useQuasar } from "quasar";
import { useRouter } from "vue-router";

export default defineComponent({
   name: "login",
   setup() {
      const $q = useQuasar();
      const isPwd = ref(true);
      const password = ref(null);
      const email = ref(null);
      const store = useStore();
      const router = useRouter();

      const onSubmit = async () => {
         const result = await store.dispatch("authModel/signin", {
            email: email.value,
            password: password.value,
         });
         if (result && result.success) {
            router.push("/");
         } else {
            $q.notify({
               type: "negative",
               position: "top",
               message: result.message || "something broke!!",
            });
         }
      };

      return { isPwd, password, email, onSubmit };
   },
});
</script>
