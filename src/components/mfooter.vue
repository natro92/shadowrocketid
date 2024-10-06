<script setup>
import { ref, onMounted } from 'vue';

const footerRef = ref(null);

onMounted(() => {
    const isMobile = window.matchMedia("(max-width: 768px)").matches;
    if (!isMobile) {
        footerRef.value.style.position = 'absolute';
        footerRef.value.style.bottom = '0';
        footerRef.value.style.width = '100%';
    }
});
</script>

<template>
    <footer ref="footerRef" class="footer footer-center bg-base-300 text-base-content p-4">
        <div class="custom-font-1 text-xs text-gray-600 flex flex-col items-center justify-center gap-2">
            <div>
                为保护你的隐私，请勿相信任何广告信息。账号内容仅供参考。
            </div>
            <div>
                ©2020-2024 公众号：源讯汇
            </div>
        </div>
    </footer>
</template>

<style scoped>

</style>
