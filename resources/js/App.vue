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
                    <a href="#alive">Оживайки</a>
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
                <div class="floating-bg" :style="floatingBgStyle" aria-hidden="true">
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

                <div class="container hero-shell">
                    <div class="hero-copy">
                        <span class="eyebrow">Новинки и праздничные предложения</span>
                        <h1>Подарки, которые хочется дарить сразу</h1>
                        <p>
                            Большой акцент на реальных подарках, красивой упаковке и персональных деталях
                            для тёплых поздравлений.
                        </p>
                        <a href="#contacts" class="hero-cta">Заказать</a>
                    </div>

                    <div class="photo-slider" :style="heroParallaxStyle">
                        <div
                            v-for="(slide, index) in slides"
                            :key="slide.src"
                            class="photo-slide"
                            :class="{ active: activeSlide === index }"
                        >
                            <img
                                v-if="!failedSlides[index]"
                                :src="slide.src"
                                :alt="slide.alt"
                                @error="markSlideError(index)"
                            />
                            <div v-else class="slide-placeholder">
                                <div class="slide-placeholder-card slide-placeholder-card-main"></div>
                                <div class="slide-placeholder-card slide-placeholder-card-side"></div>
                                <div class="slide-placeholder-ribbon"></div>
                            </div>

                            <div class="hero-slide-overlay">
                                <div class="hero-slide-meta">
                                    <span>{{ slide.kicker }}</span>
                                    <strong>{{ slide.caption }}</strong>
                                </div>
                            </div>
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

            <section id="alive" class="alive-section reveal" data-reveal>
                <div class="container">
                    <div class="alive-card">
                        <div class="alive-text">
                            <span class="section-label">Новинка</span>
                            <h2>Сувениры-оживайки</h2>
                            <p>
                                Персональные подарки, в которых фото и видео превращаются в тёплый
                                интерактивный сюрприз. Такой формат хорошо подходит для семейных
                                праздников, памятных дат и небольших особенных подарков.
                            </p>
                            <p>
                                Вы отправляете материалы, а мы аккуратно встраиваем их в готовый
                                продукт, чтобы подарок выглядел личным, современным и по-настоящему
                                запоминающимся.
                            </p>

                            <ul class="alive-features">
                                <li v-for="item in aliveFeatures" :key="item.title">
                                    <span class="feature-icon" aria-hidden="true">
                                        <svg viewBox="0 0 24 24">
                                            <path d="M20 7L10 17l-6-6"></path>
                                        </svg>
                                    </span>
                                    <span>{{ item.title }}</span>
                                </li>
                            </ul>

                            <a href="#contacts" class="alive-btn">Обсудить заказ</a>
                        </div>

                        <div class="alive-gallery">
                            <div class="alive-main-photo-wrap">
                                <div class="alive-photo-badge">QR + Фото + Видео</div>

                                <div class="alive-main-photo">
                                    <img
                                        v-if="!failedAliveImages[0]"
                                        :src="aliveImages[0].src"
                                        :alt="aliveImages[0].alt"
                                        @error="markAliveImageError(0)"
                                    />
                                    <div v-else class="alive-photo-placeholder alive-photo-placeholder-main">
                                        <svg viewBox="0 0 24 24" aria-hidden="true">
                                            <path d="M4 6h16a2 2 0 0 1 2 2v8a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2Z"></path>
                                            <path d="M8 14l2.5-2.5 2.5 2.5 3.5-3.5 3 3"></path>
                                            <circle cx="9" cy="10" r="1.2"></circle>
                                        </svg>
                                        <span>Фото изделия</span>
                                    </div>
                                </div>
                            </div>

                            <div class="alive-info-cards">
                                <article v-for="card in aliveSteps" :key="card.title" class="alive-info-card">
                                    <div class="alive-info-icon">{{ card.icon }}</div>
                                    <div class="alive-info-copy">
                                        <h4>{{ card.title }}</h4>
                                        <p>{{ card.text }}</p>
                                    </div>
                                </article>
                            </div>
                        </div>
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
                            <a href="#alive">Сувениры-оживайки</a>
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

const slides = [
    {
        src: '/images/slide-1.jpg',
        alt: 'Праздничный сладкий подарок',
        kicker: 'Подарочные наборы',
        caption: 'Сладкие композиции для тёплых поздравлений',
    },
    {
        src: '/images/slide-2.jpg',
        alt: 'Персональный сувенир wish gift',
        kicker: 'Индивидуальный заказ',
        caption: 'Сувениры и подарки под ваше событие',
    },
    {
        src: '/images/slide-3.jpg',
        alt: 'Праздничная коробка с конфетами',
        kicker: 'Нежная упаковка',
        caption: 'Красивые детали и аккуратная подача',
    },
];

const floatingItems = [
    { emoji: '🍬', top: '11%', left: '6%', delay: '0s' },
    { emoji: '🎁', top: '16%', left: '92%', delay: '0.8s' },
    { emoji: '💝', top: '76%', left: '7%', delay: '1.4s' },
    { emoji: '✨', top: '63%', left: '91%', delay: '0.5s' },
];

const aliveFeatures = [
    { title: 'Майка-оживайка' },
    { title: 'Фото-оживайка' },
    { title: 'Кружка' },
    { title: 'Брелок' },
    { title: 'Шоколад' },
    { title: 'Трек-пластинка' },
];

const aliveSteps = [
    { icon: '📷', title: 'Ваше фото', text: 'Отправляете фотографию' },
    { icon: '🎥', title: 'Ваше видео', text: 'Отправляете короткое видео' },
    { icon: '🎁', title: 'Готовый подарок', text: 'Получаете персональный сувенир' },
];

const aliveImages = [
    { src: '/images/alive-1.jpg', alt: 'Сувенир-оживайка с фото' },
];

const logoSrc = '/images/logo.png';

const activeSlide = ref(0);
const failedSlides = ref({});
const failedAliveImages = ref({});
const heroRef = ref(null);
const parallax = ref({ x: 0, y: 0 });

let autoplayId = null;
let observer = null;

const heroParallaxStyle = computed(() => ({
    transform: `translate3d(${parallax.value.x}px, ${parallax.value.y}px, 0)`,
}));

const floatingBgStyle = computed(() => ({
    transform: `translate3d(${parallax.value.x * -0.7}px, ${parallax.value.y * -0.7}px, 0)`,
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

function markAliveImageError(index) {
    failedAliveImages.value = {
        ...failedAliveImages.value,
        [index]: true,
    };
}

function onHeroMove(event) {
    const element = heroRef.value;
    if (!element) {
        return;
    }

    const rect = element.getBoundingClientRect();
    const offsetX = ((event.clientX - rect.left) / rect.width - 0.5) * 10;
    const offsetY = ((event.clientY - rect.top) / rect.height - 0.5) * 10;

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
        radial-gradient(circle at top left, rgba(233, 138, 172, 0.12), transparent 26%),
        linear-gradient(180deg, #fffafc 0%, #fff4f6 52%, #fff8fb 100%);
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
    transition: opacity 0.6s ease, transform 0.6s ease;
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
    backdrop-filter: blur(18px);
    background: rgba(255, 250, 252, 0.68);
    border-bottom: 1px solid rgba(122, 31, 70, 0.08);
    box-shadow: 0 8px 24px rgba(109, 31, 70, 0.04);
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
    transition: transform 0.35s ease, background 0.35s ease, color 0.35s ease;
}

.nav a:hover,
.header-phone:hover {
    background: rgba(255, 255, 255, 0.84);
    color: #611635;
    transform: translateY(-1px);
}

.hero {
    position: relative;
    min-height: 610px;
    padding: 32px 0 14px;
    display: flex;
    align-items: center;
}

.hero::before {
    content: '';
    position: absolute;
    inset: 0;
    background:
        radial-gradient(circle at 15% 20%, rgba(215, 131, 180, 0.12), transparent 20%),
        radial-gradient(circle at 85% 16%, rgba(165, 60, 115, 0.08), transparent 18%);
    pointer-events: none;
}

.floating-bg {
    position: absolute;
    inset: 0;
    pointer-events: none;
    overflow: hidden;
    transition: transform 0.45s ease;
}

.floating-item {
    position: absolute;
    font-size: clamp(1.25rem, 2vw, 1.8rem);
    opacity: 0.14;
    animation: floatItem 6s ease-in-out infinite;
}

.hero-shell {
    position: relative;
    z-index: 1;
    display: grid;
    gap: 18px;
    animation: heroFadeUp 0.8s ease both;
}

.hero-copy {
    display: grid;
    gap: 12px;
    max-width: 580px;
}

.eyebrow {
    display: inline-flex;
    width: fit-content;
    padding: 8px 14px;
    border-radius: 999px;
    background: rgba(248, 237, 245, 0.92);
    color: #7b173f;
    font-size: 0.88rem;
    font-weight: 700;
    letter-spacing: 0.02em;
}

.hero-copy h1 {
    margin: 0;
    color: #6d1f46;
    font-size: clamp(2.1rem, 4vw, 3.8rem);
    line-height: 1;
}

.hero-copy p {
    margin: 0;
    max-width: 520px;
    color: #7f5670;
    font-size: 0.98rem;
    line-height: 1.7;
}

.hero-cta {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: fit-content;
    min-height: 54px;
    padding: 0 30px;
    border-radius: 999px;
    background: linear-gradient(135deg, #6d1f46, #a53c73);
    color: #ffffff;
    font-weight: 700;
    box-shadow: 0 16px 36px rgba(109, 31, 70, 0.18);
    transition: transform 0.35s ease, box-shadow 0.35s ease, opacity 0.35s ease;
}

.hero-cta:hover {
    transform: translateY(-2px);
    box-shadow: 0 20px 42px rgba(109, 31, 70, 0.24);
}

.photo-slider {
    position: relative;
    width: 100%;
    max-width: 1120px;
    height: 500px;
    border-radius: 38px;
    overflow: hidden;
    border: 1px solid rgba(122, 31, 70, 0.1);
    background: #ffffff;
    box-shadow: 0 24px 64px rgba(109, 31, 70, 0.12);
    transition: transform 0.4s ease, box-shadow 0.4s ease;
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
        linear-gradient(135deg, #fff7fa 0%, #f9eef5 54%, #f3d9e6 100%);
}

.slide-placeholder-card {
    position: absolute;
    border-radius: 28px;
    background: rgba(255, 255, 255, 0.9);
    border: 1px solid rgba(165, 60, 115, 0.12);
    box-shadow: 0 18px 38px rgba(109, 31, 70, 0.08);
}

.slide-placeholder-card-main {
    width: 38%;
    height: 62%;
    left: 11%;
    top: 20%;
    transform: rotate(-4deg);
}

.slide-placeholder-card-side {
    width: 42%;
    height: 70%;
    right: 11%;
    top: 14%;
    transform: rotate(5deg);
}

.slide-placeholder-ribbon {
    position: absolute;
    width: 18%;
    height: 18px;
    left: 41%;
    top: 48%;
    border-radius: 999px;
    background: rgba(165, 60, 115, 0.16);
}

.hero-slide-overlay {
    position: absolute;
    inset: auto 0 0 0;
    padding: 22px 24px;
    background: linear-gradient(180deg, transparent, rgba(58, 16, 35, 0.52));
}

.hero-slide-meta {
    display: grid;
    gap: 6px;
    color: #ffffff;
}

.hero-slide-meta span {
    font-size: 0.82rem;
    letter-spacing: 0.06em;
    text-transform: uppercase;
    opacity: 0.86;
}

.hero-slide-meta strong {
    font-size: 1.08rem;
    font-weight: 600;
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

.alive-section {
    padding: 8px 0 24px;
}

.alive-card {
    display: grid;
    grid-template-columns: 40% 60%;
    gap: 34px;
    align-items: center;
    padding: 34px;
    border-radius: 32px;
    background: #ffffff;
    border: 1px solid rgba(215, 131, 180, 0.2);
    box-shadow: 0 20px 60px rgba(109, 31, 70, 0.08);
}

.alive-text {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    gap: 18px;
}

.section-label {
    display: inline-flex;
    align-items: center;
    padding: 8px 12px;
    border-radius: 999px;
    background: #f8edf5;
    border: 1px solid rgba(165, 60, 115, 0.12);
    color: #a53c73;
    font-size: 0.82rem;
    font-weight: 700;
    letter-spacing: 0.04em;
    text-transform: uppercase;
}

.alive-text h2 {
    margin: 0;
    color: #6d1f46;
    font-size: clamp(1.8rem, 2.5vw, 2.4rem);
    line-height: 1.08;
}

.alive-text p {
    margin: 0;
    max-width: 430px;
    color: #7f5670;
    line-height: 1.8;
}

.alive-features {
    display: grid;
    gap: 11px;
    padding: 0;
    margin: 0;
    list-style: none;
}

.alive-features li {
    display: flex;
    align-items: center;
    gap: 12px;
    color: #6d1f46;
}

.feature-icon {
    width: 26px;
    height: 26px;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    background: #f8edf5;
    flex-shrink: 0;
}

.feature-icon svg {
    width: 14px;
    height: 14px;
    fill: none;
    stroke: #a53c73;
    stroke-width: 2.1;
    stroke-linecap: round;
    stroke-linejoin: round;
}

.alive-btn {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    min-height: 46px;
    padding: 0 20px;
    border-radius: 999px;
    background: #fdf5f8;
    border: 1px solid rgba(165, 60, 115, 0.14);
    color: #6d1f46;
    font-weight: 600;
    transition: transform 0.35s ease, background 0.35s ease, color 0.35s ease, border-color 0.35s ease;
}

.alive-btn:hover {
    transform: translateY(-2px);
    background: #ffffff;
    color: #a53c73;
    border-color: rgba(165, 60, 115, 0.22);
}

.alive-gallery {
    display: grid;
    gap: 24px;
    min-width: 0;
    padding: 32px 36px;
}

.alive-main-photo-wrap {
    position: relative;
    width: calc(100% - 32px);
    margin: 0 auto;
}

.alive-photo-badge {
    position: absolute;
    top: 24px;
    left: 24px;
    z-index: 2;
    display: inline-flex;
    align-items: center;
    padding: 10px 14px;
    border-radius: 999px;
    background: rgba(255, 255, 255, 0.92);
    border: 1px solid rgba(165, 60, 115, 0.12);
    color: #6d1f46;
    font-size: 0.82rem;
    font-weight: 700;
    box-shadow: 0 12px 28px rgba(109, 31, 70, 0.1);
}

.alive-main-photo {
    overflow: hidden;
    height: 360px;
    padding: 20px;
    border-radius: 24px;
    background:
        linear-gradient(180deg, rgba(253, 245, 248, 0.9), rgba(255, 255, 255, 0.98));
    border: 1px solid rgba(165, 60, 115, 0.08);
    box-shadow: 0 20px 40px rgba(109, 31, 70, 0.08);
}

.alive-main-photo img,
.alive-photo-placeholder {
    width: 100%;
    height: 100%;
    display: block;
    object-fit: cover;
}

.alive-photo-placeholder {
    position: relative;
    background: linear-gradient(145deg, #fffdfd, #f8edf5 58%, #f2d8e6 100%);
}

.alive-photo-placeholder-main {
    display: grid;
    place-items: center;
    gap: 12px;
    color: #8d5a76;
    text-align: center;
}

.alive-photo-placeholder-main svg {
    width: 42px;
    height: 42px;
    fill: none;
    stroke: #a53c73;
    stroke-width: 1.8;
    stroke-linecap: round;
    stroke-linejoin: round;
}

.alive-photo-placeholder-main span {
    position: relative;
    z-index: 1;
    font-weight: 600;
}

.alive-photo-placeholder::before {
    content: '';
    position: absolute;
    inset: 16px;
    border-radius: 20px;
    border: 1px solid rgba(165, 60, 115, 0.08);
    background:
        linear-gradient(180deg, rgba(165, 60, 115, 0.08), rgba(165, 60, 115, 0.02)),
        linear-gradient(90deg, rgba(255, 255, 255, 0.86), rgba(255, 255, 255, 0.42));
}

.alive-photo-placeholder::after {
    content: '';
    position: absolute;
    width: 34%;
    height: 12px;
    left: 16px;
    bottom: 16px;
    border-radius: 999px;
    background: rgba(165, 60, 115, 0.08);
}

.alive-photo-placeholder-main::before {
    inset: 20px;
}

.alive-info-cards {
    display: grid;
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 16px;
    width: calc(100% - 32px);
    margin: 0 auto;
}

.alive-info-card {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 14px;
    min-height: 180px;
    padding: 28px 24px;
    border-radius: 20px;
    background: #ffffff;
    border: 1px solid rgba(215, 131, 180, 0.16);
    box-shadow: 0 14px 30px rgba(109, 31, 70, 0.05);
    text-align: center;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.alive-info-card:hover {
    transform: translateY(-6px);
    box-shadow: 0 20px 40px rgba(109, 31, 70, 0.1);
}

.alive-info-icon {
    width: 56px;
    height: 56px;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    background: #f8edf5;
    font-size: 24px;
    flex-shrink: 0;
    box-shadow: 0 8px 20px rgba(109, 31, 70, 0.08);
    transition: transform 0.3s ease;
}

.alive-info-card:hover .alive-info-icon {
    transform: scale(1.08);
}

.alive-info-copy {
    display: grid;
    gap: 10px;
    justify-items: center;
}

.alive-info-copy h4 {
    margin: 0;
    color: #6d1f46;
    font-size: 20px;
    font-weight: 700;
}

.alive-info-copy p {
    margin: 0;
    color: #7b6a76;
    font-size: 0.95rem;
    line-height: 1.6;
}

.site-footer {
    width: 100%;
    margin-top: 10px;
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
    transition: 0.35s;
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
    transition: 0.35s;
}

@keyframes heroFadeUp {
    0% {
        opacity: 0;
        transform: translateY(18px);
    }
    100% {
        opacity: 1;
        transform: translateY(0);
    }
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

    .alive-card {
        grid-template-columns: 1fr;
        gap: 28px;
    }

    .alive-gallery {
        min-height: auto;
    }

    .footer-grid {
        grid-template-columns: 1fr;
        gap: 36px;
    }
}

@media (max-width: 720px) {
    .hero {
        min-height: auto;
        padding-top: 28px;
    }

    .hero-copy {
        gap: 12px;
    }

    .photo-slider {
        height: 400px;
        border-radius: 28px;
    }

    .hero-slide-overlay {
        padding: 18px 18px 20px;
    }

    .alive-card {
        padding: 24px 22px;
        border-radius: 26px;
    }

    .alive-main-photo {
        height: 280px;
    }

    .alive-info-cards {
        grid-template-columns: 1fr;
    }

    .alive-gallery {
        padding: 8px 0 0;
        gap: 14px;
    }

    .alive-main-photo-wrap,
    .alive-info-cards {
        width: 100%;
        margin: 0;
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
