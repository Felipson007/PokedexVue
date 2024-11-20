<template>
    <button v-show="visible" class="btn btn-primary scroll-to-top" @click="scrollToTop">
        ↑
    </button>
</template>

<script>
export default {
    name: "ScrollToTopButton",
    data() {
        return {
            visible: false,
        };
    },
    methods: {
        scrollToTop() {
            window.scrollTo({ top: 0, behavior: "smooth" });
        },
        handleScroll() {
            this.visible = window.scrollY > 200;
        },
    },
    mounted() {
        window.addEventListener("scroll", this.handleScroll);
    },
    beforeUnmount() {
        window.removeEventListener("scroll", this.handleScroll);
    },
};
</script>

<style scoped>
.scroll-to-top {
    position: fixed;
    bottom: 20px;
    right: 20px;
    z-index: 1000;
    background-color: #6c757d;
    color: white;
    border: none;
    border-radius: 50%;
    width: 50px;
    height: 50px;
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    cursor: pointer;
    font-size: 18px;
}

@media (max-width: 768px) {
    .scroll-to-top {
        font-size: 12px;
        padding: 8px 12px;
    }
}
</style>