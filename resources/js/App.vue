<template>
    <div class="page-shell">
        <header class="site-header reveal" data-reveal>
            <div class="container header-row">
                <a href="#top" class="brand logo">
                    <img :src="logoSrc" alt="wish gift" class="logo-img" />
                    <span class="brand-copy">
                        wish_gift
                        <small>Конфеты, сувениры, праздники</small>
                    </span>
                </a>

                <nav class="nav">
                    <a href="#top">Главная</a>
                    <a href="#home">Новинки</a>
                    <a href="#contacts">Контакты</a>
                </nav>

                <a class="header-phone" href="tel:+37100000000">+371 00 000 000</a>
            </div>
        </header>

        <main id="top">
            <section
                id="home"
                ref="heroRef"
                class="hero reveal"
                data-reveal
                @mousemove="onHeroMove"
                @mouseleave="resetHeroParallax"
            >
                <div class="floating-bg" aria-hidden="true">
                    <span
                        v-for="item in floatingItems"
                        :key="item.emoji + item.top"
                        class="floating-item"
                        :style="{
                            top: item.top,
                            left: item.left,
                            animationDelay: item.delay,
                        }"
                    >
                        {{ item.emoji }}
                    </span>
                </div>

                <div class="container hero-content">
                    <span class="eyebrow">Новинки и праздничные предложения</span>

                    <div class="photo-slider" :style="heroParallaxStyle">
                        <div
                            v-for="(slide, index) in slides"
                            :key="slide"
                            class="photo-slide"
                            :class="{ active: activeSlide === index }"
                        >
                            <img
                                v-if="!failedSlides[index]"
                                :src="slide"
                                alt="wish gift slide"
                                @error="markSlideError(index)"
                            />
                            <div v-else class="slide-placeholder"></div>
                        </div>
                    </div>

                    <div class="slider-dots" aria-label="Переключение слайдов">
                        <button
                            v-for="(_, index) in slides"
                            :key="index"
                            class="dot"
                            :class="{ active: index === activeSlide }"
                            type="button"
                            :aria-label="`Слайд ${index + 1}`"
                            @click="setSlide(index)"
                        />
                    </div>
                </div>
            </section>

        </main>

        <footer id="contacts" class="site-footer reveal" data-reveal>
            <div class="container footer-wrap">
                <div class="footer-grid">
                    <div class="footer-brand">
                        <div class="footer-logo-line">
                            <img :src="logoSrc" alt="wish gift" class="footer-logo-img" />
                            <span class="footer-brand-name">wish_gift</span>
                        </div>
                        <p class="footer-description">
                            Подарки из конфет, сувениры и персональные подарки для любого праздника.
                        </p>
                    </div>

                    <div class="footer-column">
                        <h3 class="footer-title">Навигация</h3>
                        <nav class="footer-links" aria-label="Footer navigation">
                            <a href="#top">Главная</a>
                            <a href="#home">Каталог</a>
                            <a href="#home">Сувениры-оживайки</a>
                            <a href="#contacts">О нас</a>
                            <a href="#contacts">Заказать</a>
                        </nav>
                    </div>

                    <div class="footer-column">
                        <h3 class="footer-title">Связаться</h3>
                        <div class="footer-contacts">
                            <a href="tel:+37100000000" class="footer-contact-link">
                                <span class="footer-contact-icon">☎</span>
                                <span>+371 XX XXX XXX</span>
                            </a>
                            <a href="mailto:info@wishgift.lv" class="footer-contact-link">
                                <span class="footer-contact-icon">✉</span>
                                <span>info@wishgift.lv</span>
                            </a>
                            <div class="footer-contact-link footer-contact-static">
                                <span class="footer-contact-icon">📍</span>
                                <span>Латвия</span>
                            </div>
                        </div>

                        <div class="footer-socials" aria-label="Social links">
                            <a href="#" class="social-icon" aria-label="Instagram">
                                <svg viewBox="0 0 24 24" aria-hidden="true">
                                    <rect x="3" y="3" width="18" height="18" rx="5"></rect>
                                    <circle cx="12" cy="12" r="4.2"></circle>
                                    <circle cx="17.2" cy="6.8" r="1.2"></circle>
                                </svg>
                            </a>
                            <a href="#" class="social-icon" aria-label="Facebook">
                                <svg viewBox="0 0 24 24" aria-hidden="true">
                                    <path d="M13.4 21v-7.3h2.5l.4-3h-2.9V8.8c0-.9.2-1.5 1.5-1.5h1.6V4.6c-.3 0-1.2-.1-2.3-.1-2.3 0-3.8 1.4-3.8 4v2.2H8v3h2.4V21h3z"></path>
                                </svg>
                            </a>
                            <a href="#" class="social-icon" aria-label="TikTok">
                                <svg viewBox="0 0 24 24" aria-hidden="true">
                                    <path d="M14.7 3c.4 2 1.5 3.3 3.3 4.1v2.7c-1.3 0-2.5-.4-3.5-1.1v5.7c0 3-2.4 5.4-5.4 5.4s-5.1-2.4-5.1-5.4 2.4-5.2 5.1-5.2c.3 0 .7 0 1 .1v2.8a3 3 0 0 0-.9-.1c-1.4 0-2.5 1.1-2.5 2.5s1.1 2.6 2.5 2.6 2.6-1.1 2.6-2.6V3h2.9z"></path>
                                </svg>
                            </a>
                        </div>
                    </div>
                </div>

                <div class="footer-bottom">
                    <span>© 2025 wish_gift. Все права защищены.</span>
                    <div class="footer-bottom-links">
                        <a href="#">Политика конфиденциальности</a>
                        <a href="#">Условия использования</a>
                    </div>
                </div>
            </div>
        </footer>
    </div>
</template>

<script setup>
import { computed, onBeforeUnmount, onMounted, ref } from 'vue';

const slides = ['/images/slide-1.jpg', '/images/slide-2.jpg', '/images/slide-3.jpg'];

const floatingItems = [
    { emoji: '🍬', top: '9%', left: '6%', delay: '0s' },
    { emoji: '🎁', top: '17%', left: '91%', delay: '0.8s' },
    { emoji: '💝', top: '72%', left: '8%', delay: '1.4s' },
    { emoji: '✨', top: '61%', left: '90%', delay: '0.5s' },
    { emoji: '🍬', top: '35%', left: '3%', delay: '1.1s' },
    { emoji: '✨', top: '42%', left: '95%', delay: '1.8s' },
];

const logoSrc = '/images/logo.png';

const activeSlide = ref(0);
const failedSlides = ref({});
const heroRef = ref(null);
const parallax = ref({ x: 0, y: 0 });

let autoplayId = null;
let observer = null;

const heroParallaxStyle = computed(() => ({
    transform: `translate3d(${parallax.value.x}px, ${parallax.value.y}px, 0)`,
}));

function setSlide(index) {
    activeSlide.value = index;
    restartAutoplay();
}

function nextSlide() {
    activeSlide.value = (activeSlide.value + 1) % slides.length;
}

function restartAutoplay() {
    if (autoplayId) {
        window.clearInterval(autoplayId);
    }

    autoplayId = window.setInterval(() => {
        nextSlide();
    }, 4000);
}

function markSlideError(index) {
    failedSlides.value = {
        ...failedSlides.value,
        [index]: true,
    };
}

function onHeroMove(event) {
    const element = heroRef.value;
    if (!element) {
        return;
    }

    const rect = element.getBoundingClientRect();
    const offsetX = ((event.clientX - rect.left) / rect.width - 0.5) * 12;
    const offsetY = ((event.clientY - rect.top) / rect.height - 0.5) * 12;

    parallax.value = {
        x: offsetX,
        y: offsetY,
    };
}

function resetHeroParallax() {
    parallax.value = { x: 0, y: 0 };
}

onMounted(() => {
    restartAutoplay();

    const items = document.querySelectorAll('[data-reveal]');
    observer = new IntersectionObserver(
        (entries) => {
            entries.forEach((entry) => {
                if (entry.isIntersecting) {
                    entry.target.classList.add('is-visible');
                    observer?.unobserve(entry.target);
                }
            });
        },
        {
            threshold: 0.18,
            rootMargin: '0px 0px -40px 0px',
        }
    );

    items.forEach((item) => observer?.observe(item));
});

onBeforeUnmount(() => {
    if (autoplayId) {
        window.clearInterval(autoplayId);
    }

    observer?.disconnect();
});
</script>

<style scoped>
:global(*) {
    box-sizing: border-box;
}

:global(html) {
    scroll-behavior: smooth;
}

:global(body) {
    margin: 0;
    font-family: 'Instrument Sans', 'Segoe UI', sans-serif;
    background:
        radial-gradient(circle at top left, rgba(233, 138, 172, 0.18), transparent 24%),
        radial-gradient(circle at right top, rgba(182, 143, 217, 0.18), transparent 28%),
        linear-gradient(180deg, #fff9f7 0%, #fff2f4 54%, #fff7f2 100%);
    color: #4b233c;
}

:global(a) {
    color: inherit;
    text-decoration: none;
}

:global(button) {
    font: inherit;
}

.page-shell {
    min-height: 100vh;
    overflow: hidden;
}

.container {
    width: min(1120px, calc(100% - 32px));
    margin: 0 auto;
}

.reveal {
    opacity: 0;
    transform: translateY(28px);
    transition: opacity 0.75s ease, transform 0.75s ease;
}

.reveal.is-visible {
    opacity: 1;
    transform: translateY(0);
}

.site-header {
    position: sticky;
    top: 0;
    z-index: 20;
    padding: 16px 0;
    backdrop-filter: blur(16px);
    background: rgba(255, 248, 246, 0.72);
    border-bottom: 1px solid rgba(122, 31, 70, 0.08);
}

.header-row {
    display: grid;
    grid-template-columns: auto 1fr auto;
    gap: 20px;
    align-items: center;
}

.brand,
.logo {
    display: inline-flex;
    align-items: center;
    gap: 12px;
}

.logo-img {
    width: 52px;
    height: 52px;
    object-fit: contain;
    border-radius: 14px;
}

.brand-copy {
    display: grid;
    gap: 2px;
    font-size: 1rem;
    font-weight: 700;
    color: #621938;
}

.brand-copy small {
    font-size: 0.78rem;
    font-weight: 500;
    color: #8b5f77;
}

.nav {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    gap: 8px;
}

.nav a,
.header-phone {
    padding: 10px 14px;
    border-radius: 999px;
    color: #74455e;
    transition: transform 0.25s ease, background 0.25s ease, color 0.25s ease;
}

.nav a:hover,
.header-phone:hover {
    background: rgba(255, 255, 255, 0.78);
    color: #611635;
    transform: translateY(-1px);
}

.hero {
    position: relative;
    min-height: 720px;
    padding: 42px 0 24px;
    display: flex;
    align-items: center;
}

.hero::before {
    content: '';
    position: absolute;
    inset: 26px 0 auto;
    height: 620px;
    background: linear-gradient(120deg, #7b173f, #b98bd8, #ee95b4, #f7d5d1);
    background-size: 260% 260%;
    animation: gradientShift 14s ease infinite;
    opacity: 0.17;
    filter: blur(12px);
}

.floating-bg {
    position: absolute;
    inset: 0;
    pointer-events: none;
    overflow: hidden;
}

.floating-item {
    position: absolute;
    font-size: clamp(1.35rem, 2vw, 2rem);
    opacity: 0.18;
    animation: floatItem 5.5s ease-in-out infinite;
    filter: drop-shadow(0 10px 24px rgba(123, 23, 63, 0.08));
}

.hero-content {
    position: relative;
    z-index: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 22px;
    width: 100%;
}

.eyebrow {
    display: inline-flex;
    padding: 8px 14px;
    border-radius: 999px;
    background: rgba(248, 232, 237, 0.95);
    color: #7b173f;
    font-size: 0.88rem;
    font-weight: 700;
    letter-spacing: 0.02em;
}

.photo-slider {
    border: 1px solid rgba(122, 31, 70, 0.1);
    background: rgba(255, 255, 255, 0.76);
    box-shadow: 0 18px 45px rgba(123, 23, 63, 0.09);
    backdrop-filter: blur(16px);
}

.photo-slider {
    position: relative;
    width: 100%;
    max-width: 1120px;
    height: 560px;
    border-radius: 36px;
    overflow: hidden;
    transition: transform 0.3s ease;
}

.photo-slide {
    position: absolute;
    inset: 0;
    opacity: 0;
    transform: scale(1.04);
    transition: opacity 0.8s ease, transform 0.8s ease;
}

.photo-slide.active {
    opacity: 1;
    transform: scale(1);
    z-index: 1;
}

.photo-slide img,
.slide-placeholder {
    width: 100%;
    height: 100%;
    display: block;
    object-fit: cover;
}

.slide-placeholder {
    position: relative;
    background:
        radial-gradient(circle at 20% 20%, rgba(255, 255, 255, 0.42), transparent 22%),
        radial-gradient(circle at 80% 30%, rgba(255, 255, 255, 0.34), transparent 20%),
        linear-gradient(135deg, #7b173f 0%, #b98bd8 52%, #ee95b4 100%);
}

.slide-placeholder::before,
.slide-placeholder::after {
    content: '';
    position: absolute;
    border-radius: 999px;
    background: rgba(255, 255, 255, 0.18);
}

.slide-placeholder::before {
    width: 240px;
    height: 240px;
    top: 12%;
    right: 8%;
}

.slide-placeholder::after {
    width: 320px;
    height: 320px;
    left: 7%;
    bottom: -12%;
}

.slider-dots {
    display: flex;
    justify-content: center;
    gap: 10px;
}

.dot {
    width: 12px;
    height: 12px;
    border-radius: 50%;
    border: 0;
    cursor: pointer;
    background: rgba(123, 23, 63, 0.2);
    transition: width 0.25s ease, background 0.25s ease, transform 0.25s ease;
}

.dot.active {
    width: 34px;
    border-radius: 999px;
    background: #7b173f;
}

.site-footer {
    width: 100%;
    margin-top: 6px;
    padding: 80px 0 30px;
    background: linear-gradient(135deg, #fff8fb, #f8edf5);
    border-top: 1px solid rgba(165, 60, 115, 0.15);
}

.footer-wrap {
    max-width: 1200px;
}

.footer-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 80px;
}

.footer-brand,
.footer-column {
    min-width: 0;
}

.footer-logo-line {
    display: flex;
    align-items: center;
    gap: 14px;
}

.footer-logo-img {
    width: 56px;
    height: 56px;
    object-fit: contain;
    border-radius: 16px;
}

.footer-brand-name {
    font-size: 1.2rem;
    font-weight: 700;
    color: #6d1f46;
}

.footer-description {
    margin: 18px 0 0;
    max-width: 320px;
    color: #7f5670;
    line-height: 1.75;
}

.footer-title {
    margin: 0 0 20px;
    font-size: 1.08rem;
    color: #6d1f46;
}

.footer-links,
.footer-contacts {
    display: flex;
    flex-direction: column;
    gap: 12px;
}

.footer-links a,
.footer-contact-link,
.footer-bottom-links a {
    color: #7f5670;
    transition: 0.3s;
}

.footer-links a:hover,
.footer-contact-link:hover,
.footer-bottom-links a:hover {
    color: #a53c73;
}

.footer-contact-link {
    display: inline-flex;
    align-items: center;
    gap: 10px;
}

.footer-contact-static {
    cursor: default;
}

.footer-contact-icon {
    color: #a53c73;
}

.footer-socials {
    display: flex;
    gap: 12px;
    margin-top: 22px;
}

.social-icon {
    width: 42px;
    height: 42px;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    background: #ffffff;
    border: 1px solid rgba(165, 60, 115, 0.12);
    color: #a53c73;
    transition: 0.3s;
}

.social-icon svg {
    width: 18px;
    height: 18px;
    fill: none;
    stroke: currentColor;
    stroke-width: 1.8;
    stroke-linecap: round;
    stroke-linejoin: round;
}

.social-icon:hover {
    background: #a53c73;
    color: #ffffff;
    transform: scale(1.06);
}

.footer-bottom {
    margin-top: 42px;
    padding-top: 22px;
    border-top: 1px solid rgba(165, 60, 115, 0.15);
    display: flex;
    justify-content: space-between;
    gap: 18px;
    flex-wrap: wrap;
    color: #7f5670;
    font-size: 0.95rem;
}

.footer-bottom-links {
    display: flex;
    gap: 22px;
    flex-wrap: wrap;
}

@keyframes gradientShift {
    0% {
        background-position: 0% 50%;
    }
    50% {
        background-position: 100% 50%;
    }
    100% {
        background-position: 0% 50%;
    }
}

@keyframes floatItem {
    0%,
    100% {
        transform: translateY(0);
    }
    50% {
        transform: translateY(-12px);
    }
}

@media (max-width: 980px) {
    .header-row {
        grid-template-columns: 1fr;
    }

    .nav {
        justify-content: flex-start;
    }

    .footer-grid {
        grid-template-columns: 1fr;
        gap: 36px;
    }
}

@media (max-width: 720px) {
    .hero {
        min-height: auto;
        padding-top: 26px;
    }

    .photo-slider {
        height: 420px;
        border-radius: 26px;
    }

    .header-phone {
        justify-self: start;
    }

    .site-footer {
        padding: 56px 0 26px;
    }

    .footer-bottom,
    .footer-bottom-links {
        flex-direction: column;
        gap: 12px;
    }
}
</style>
