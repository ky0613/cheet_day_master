<template>
  <div class="py-6 sm:py-8 lg:py-12">
    <div class="max-w-screen-2xl px-4 md:px-8 mx-auto">
      <p
        class="text-gray-800 text-2xl lg:text-3xl font-bold text-center mb-4 md:mb-8"
      >
        パスワードリセット
      </p>
      <div class="max-w-lg border rounded-lg mx-auto bg-white">
        <ValidationObserver v-slot="ObserverProps">
          <div class="flex flex-col gap-4 p-4 md:p-8">
            <ValidationProvider rules="required|email" mode="lazy">
              <div slot-scope="ProviderProps">
                <label
                  for="email"
                  class="inline-block text-gray-800 text-sm sm:text-base mb-2"
                  >メールアドレス</label
                >
                <input
                  v-model="email"
                  name="email"
                  class="w-full bg-gray-50 text-gray-800 border focus:ring ring-indigo-300 rounded outline-none transition duration-100 px-3 py-2"
                  placeholder="test@example.com"
                  autocomplete="email"
                  type="email"
                />
                <span class="text-red-500 text-center">{{
                  ProviderProps.errors[0]
                }}</span>
              </div>
            </ValidationProvider>
            <button
              type="button"
              class="block bg-orange-300 hover:bg-orange-400 disabled:bg-orange-200 focus-visible:ring ring-gray-300 text-white text-sm md:text-base font-semibold text-center rounded-lg outline-none transition duration-100 px-8 py-3"
              :disabled="ObserverProps.invalid || !ObserverProps.validated"
              @click="sendEmail"
            >
              メールを送信する
            </button>
          </div>
        </ValidationObserver>
      </div>
    </div>
  </div>
</template>

<script>
import { extend, ValidationProvider, ValidationObserver } from "vee-validate";
import { required, email } from "vee-validate/dist/rules";
import axios from "../../plugins/axios";

required.message = "必須項目です。入力してください。";
email.message = "メールの形式ではありません";

extend("required", required);
extend("email", email);

export default {
  name: "ResetPasswordCreate",
  components: {
    ValidationProvider,
    ValidationObserver,
  },
  data() {
    return {
      email: "",
    };
  },
  methods: {
    async sendEmail() {
      await axios.post("password_resets", {
        email: this.email,
      });
      this.$router.push({ name: "TopIndex" });
    },
  },
};
</script>
