<script setup>
import Mfooter from "@/components/mfooter.vue";
</script>

<template>
    <div class="min-h-screen flex flex-col items-center justify-center gap-5 p-4">
        <div>
            <div class="flex flex-col sm:flex-row items-center gap-4 sm:gap-8">
                <div class="avatar">
                    <div class="w-20 sm:w-16 rounded-full border-2">
                        <img src="./../assets/logo.png"/>
                    </div>
                </div>
                <div class="text-4xl sm:text-6xl font-bold text-blue-600 text-center">ShadowRocket-Free</div>
            </div>
        </div>

        <div class="custom-font-1 text-base sm:text-2xl text-center">
            账号资源来自于网络，请勿相信任何相关广告信息。请登录自AppStore而不是设置。
        </div>

        <div class="w-full max-w-md">
            <div
                    v-for="(account, index) in accounts"
                    :key="index"
                    class="mb-4 border rounded-md p-4 shadow-lg"
            >
                <h2 class="text-md sm:text-lg font-bold mb-2">
                    账户 {{ index + 1 }} （点击编辑栏即可复制）
                </h2>
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-2">
                    <div>
                        <label class="block mb-2 text-gray-700 font-bold">用户名:</label>
                        <input
                                type="password"
                                class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                                :value="account.email"
                                readonly
                                @click="copyToClipboard(account.email)"
                        />
                    </div>
                    <div>
                        <label class="block mb-2 text-gray-700 font-bold">密码:</label>
                        <input
                                type="password"
                                class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                                :value="account.password"
                                readonly
                                @click="copyToClipboard(account.password)"
                        />
                    </div>
                </div>
            </div>
        </div>
        <Mfooter/>
    </div>

    <div v-if="showToast" class="fixed bottom-5 right-5 toast toast-start">
        <div class="alert alert-success">
            <span>{{ toastMessage }}</span>
        </div>
    </div>
</template>

<style scoped></style>

<script>
export default {
    data() {
        return {
            accounts: [],
            error: null,
            loading: false,
            showToast: false, // 控制 tooltip 显示
            toastMessage: "已复制到剪贴板",
        };
    },
    mounted() {
        this.fetchAccounts();
    },
    methods: {
        async fetchAccounts() {
            this.loading = true;
            this.error = null;
            try {
                const response = await fetch(
                        "https://jsd.onmicrosoft.cn/gh/natro92/shadowrocketid@account/account.txt"
                );
                if (!response.ok) {
                    throw new Error("网络请求失败");
                }
                const text = await response.text();
                this.parseAccounts(text);
            } catch (error) {
                this.error = error.message;
            } finally {
                this.loading = false;
            }
        },
        parseAccounts(text) {
            const lines = text.split("\n");
            const accounts = [];

            for (let i = 0; i < lines.length; i += 2) {
                if (lines[i] && lines[i + 1]) {
                    const email = lines[i].replace("账号: ", "").trim();
                    const password = lines[i + 1].replace("密码: ", "").trim();
                    accounts.push({
                        email,
                        password,
                    });
                }
            }

            this.accounts = accounts;
        },
        copyToClipboard(text) {
            navigator.clipboard.writeText(text);
            this.showToast = true;
            console.log(this.showToast)
            setTimeout(() => {
                this.showToast = false;
            }, 1000);
        }
    },
};
</script>
