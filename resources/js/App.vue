<template>
    <div class="page-shell">
        <header class="site-header reveal" data-reveal>
            <div class="container header-row">
                <a href="#top" class="brand">
                    <span class="brand-mark">wg</span>
                    <span class="brand-copy">
                        wish_gift
                        <small>Конфеты, сувениры, праздники</small>
                    </span>
                </a>

                <nav class="nav">
                    <a href="#top">Главная</a>
                    <a href="#categories">Категории</a>
                    <a href="#products">Товары</a>
                    <a href="#alive">Оживайки</a>
                    <a href="#order">Заказать</a>
                    <a href="#contacts">Контакты</a>
                </nav>

                <a class="header-phone" href="tel:+37100000000">+371 00 000 000</a>
            </div>
        </header>

        <main id="top">
            <section
                ref="heroRef"
                class="hero reveal"
                data-reveal
                :style="heroParallaxStyle"
                @mousemove="onHeroMove"
                @mouseleave="resetHeroParallax"
            >
                <div class="container hero-grid">
                    <div class="hero-copy">
                        <span class="hero-kicker">wish_gift для тёплых событий</span>
                        <h1>{{ slides[activeSlide].title }}</h1>
                        <p>{{ slides[activeSlide].text }}</p>

                        <div class="hero-actions">
                            <button class="btn btn-primary" type="button" @click="handleOrderClick">
                                Заказать
                            </button>
                            <a class="btn btn-secondary" href="#products">Смотреть товары</a>
                        </div>

                        <div class="slider-dots" aria-label="Переключение слайдов">
                            <button
                                v-for="(slide, index) in slides"
                                :key="slide.title"
                                class="dot"
                                :class="{ active: index === activeSlide }"
                                type="button"
                                :aria-label="`Слайд ${index + 1}`"
                                @click="setSlide(index)"
                            />
                        </div>
                    </div>

                    <div class="hero-visual">
                        <div class="visual-card">
                            <div class="visual-badge">Праздничная подборка</div>
                            <div class="visual-stage">
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
                                <div class="gift-illustration">
                                    <div class="gift-box"></div>
                                    <div class="gift-lid"></div>
                                    <div class="gift-ribbon"></div>
                                </div>
                            </div>
                            <div class="visual-footer">
                                <strong>Автосмена каждые 4 секунды</strong>
                                <span>Нежный премиальный лендинг без перегруза</span>
                            </div>
                        </div>
                    </div>
                </div>
            </section>

            <section id="categories" class="section reveal" data-reveal>
                <div class="container">
                    <div class="section-head">
                        <span class="eyebrow">Категории</span>
                        <h2>Компактно и по поводу</h2>
                    </div>

                    <div class="category-grid">
                        <article v-for="category in categories" :key="category" class="soft-card category-card">
                            <span class="category-icon">✦</span>
                            <span>{{ category }}</span>
                        </article>
                    </div>
                </div>
            </section>

            <section id="products" class="section reveal" data-reveal>
                <div class="container">
                    <div class="section-head">
                        <span class="eyebrow">Популярные товары</span>
                        <h2>Четыре аккуратных фаворита</h2>
                    </div>

                    <div class="product-grid">
                        <article v-for="product in products" :key="product.name" class="soft-card product-card hover-lift">
                            <button
                                class="favorite-btn"
                                type="button"
                                :aria-label="`Добавить ${product.name} в избранное`"
                                :class="{ active: favorites.has(product.name) }"
                                @click="toggleFavorite(product.name)"
                            >
                                <span>{{ favorites.has(product.name) ? '♥' : '♡' }}</span>
                            </button>

                            <div class="product-media">
                                <div class="product-art">
                                    <div class="product-art-inner">
                                        <span>{{ product.badge }}</span>
                                    </div>
                                </div>
                            </div>

                            <div class="product-body">
                                <h3>{{ product.name }}</h3>
                                <p>{{ product.description }}</p>
                                <div class="product-bottom">
                                    <strong>{{ product.price }}</strong>
                                    <button class="btn btn-primary btn-small" type="button" @click="handleOrderClick">
                                        Заказать
                                    </button>
                                </div>
                            </div>
                        </article>
                    </div>
                </div>
            </section>

            <section id="alive" class="section reveal" data-reveal>
                <div class="container">
                    <div class="alive-card">
                        <div class="alive-copy">
                            <span class="new-badge">НОВИНКА</span>
                            <h2>Сувениры-оживайки</h2>
                            <p>Клиент отправляет нам своё фото и видео, а мы вставляем их в продукт.</p>
                        </div>

                        <div class="alive-pills">
                            <span v-for="item in aliveItems" :key="item" class="alive-pill">{{ item }}</span>
                        </div>
                    </div>
                </div>
            </section>

            <section id="order" class="section reveal" data-reveal>
                <div class="container order-grid">
                    <div class="order-steps">
                        <div class="section-head">
                            <span class="eyebrow">Как заказать</span>
                            <h2>Простой путь в четыре шага</h2>
                        </div>

                        <div class="steps-grid">
                            <article v-for="(step, index) in steps" :key="step.title" class="soft-card step-card hover-lift">
                                <span class="step-index">0{{ index + 1 }}</span>
                                <h3>{{ step.title }}</h3>
                                <p>{{ step.text }}</p>
                            </article>
                        </div>
                    </div>

                    <div class="soft-card form-card">
                        <h3>Отправить заявку</h3>
                        <form class="order-form" action="#" method="post" enctype="multipart/form-data">
                            <label>
                                <span>Имя</span>
                                <input type="text" placeholder="Ваше имя" />
                            </label>
                            <label>
                                <span>Телефон</span>
                                <input type="tel" placeholder="+371 ..." />
                            </label>
                            <label class="full">
                                <span>Комментарий</span>
                                <textarea placeholder="Напишите, какой подарок нужен"></textarea>
                            </label>
                            <label>
                                <span>Фото</span>
                                <input type="file" accept="image/*" />
                            </label>
                            <label>
                                <span>Видео</span>
                                <input type="file" accept="video/*" />
                            </label>
                            <button class="btn btn-primary full" type="button" @click="handleOrderClick">
                                Отправить заявку
                            </button>
                        </form>
                    </div>
                </div>
            </section>
        </main>

        <footer id="contacts" class="site-footer reveal" data-reveal>
            <div class="container footer-card">
                <div class="footer-top">
                    <div>
                        <span class="eyebrow">Контакты</span>
                        <h2>wish_gift</h2>
                        <p>Нежные подарки из конфет и современные сувениры для праздников, сюрпризов и особенных событий.</p>
                    </div>

                    <div class="contact-list">
                        <a href="tel:+37100000000">+371 00 000 000</a>
                        <a href="mailto:info@wishgift.lv">info@wishgift.lv</a>
                    </div>
                </div>

                <div class="social-row">
                    <a href="#">Instagram</a>
                    <a href="#">Facebook</a>
                    <a href="#">TikTok</a>
                </div>
            </div>
        </footer>
    </div>
</template>

<script setup>
import { computed, onBeforeUnmount, onMounted, ref } from 'vue';

const slides = [
    {
        title: 'Новинка: Сувениры-оживайки',
        text: 'Эмоциональные подарки, в которых фото и видео превращаются в красивый праздничный сюрприз.',
    },
    {
        title: 'Подарки из конфет на любой праздник',
        text: 'Сладкие наборы, букеты и коробки для семейных событий, корпоративов и тёплых поздравлений.',
    },
    {
        title: 'Индивидуальные заказы с вашим фото и видео',
        text: 'Персональный подарок под конкретный повод, стиль и настроение без визуального перегруза.',
    },
];

const categories = [
    'Новый год',
    '8 марта',
    'День мамы',
    'День рождения',
    'Свадьба',
    'Корпоративы',
    'Детские праздники',
    'Индивидуальные заказы',
];

const products = [
    {
        name: 'Сладкий букет',
        description: 'Аккуратная композиция из конфет для тёплого и нежного поздравления.',
        price: 'от 28 €',
        badge: '🍬',
    },
    {
        name: 'Конфетная коробка',
        description: 'Праздничная коробка со сладостями и деликатным декором.',
        price: 'от 22 €',
        badge: '🎁',
    },
    {
        name: 'Новогодний набор',
        description: 'Уютный сезонный подарок в праздничной палитре и красивой упаковке.',
        price: 'от 30 €',
        badge: '✨',
    },
    {
        name: 'Индивидуальный набор',
        description: 'Персональный вариант с вашим настроением, фото и особыми деталями.',
        price: 'от 38 €',
        badge: '💝',
    },
];

const aliveItems = [
    'Майка-оживайка',
    'Фото-оживайка',
    'Оживи кружку',
    'Брелок',
    'Открытка',
    'Шоколад',
    'Трек-пластинка',
];

const steps = [
    { title: 'Выберите товар', text: 'Смотрите готовые позиции или ориентируйтесь на категорию.' },
    { title: 'Свяжитесь с нами', text: 'Напишите или позвоните, чтобы уточнить детали заказа.' },
    { title: 'Отправьте фото/видео', text: 'Для оживающих сувениров прикрепите нужные материалы.' },
    { title: 'Получите готовый подарок', text: 'Мы оформим заказ и подготовим красивую финальную подачу.' },
];

const floatingItems = [
    { emoji: '🍬', top: '10%', left: '12%', delay: '0s' },
    { emoji: '🎁', top: '18%', left: '74%', delay: '0.9s' },
    { emoji: '💝', top: '70%', left: '14%', delay: '1.3s' },
    { emoji: '✨', top: '62%', left: '76%', delay: '0.4s' },
];

const activeSlide = ref(0);
const favorites = ref(new Set());
const heroRef = ref(null);
const parallax = ref({ x: 0, y: 0 });

let autoplayId = null;
let observer = null;

const heroParallaxStyle = computed(() => ({
    '--mx': `${parallax.value.x}px`,
    '--my': `${parallax.value.y}px`,
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

function onHeroMove(event) {
    const element = heroRef.value;
    if (!element) {
        return;
    }

    const rect = element.getBoundingClientRect();
    const offsetX = ((event.clientX - rect.left) / rect.width - 0.5) * 20;
    const offsetY = ((event.clientY - rect.top) / rect.height - 0.5) * 20;

    parallax.value = {
        x: offsetX,
        y: offsetY,
    };
}

function resetHeroParallax() {
    parallax.value = { x: 0, y: 0 };
}

function toggleFavorite(name) {
    const next = new Set(favorites.value);

    if (next.has(name)) {
        next.delete(name);
    } else {
        next.add(name);
    }

    favorites.value = next;
}

function bounceButton(button) {
    if (!(button instanceof HTMLElement)) {
        return;
    }

    button.classList.remove('is-bouncing');
    void button.offsetWidth;
    button.classList.add('is-bouncing');
}

function handleOrderClick(event) {
    bounceButton(event.currentTarget);
    document.querySelector('#order')?.scrollIntoView({ behavior: 'smooth', block: 'start' });
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

:global(button),
:global(input),
:global(textarea) {
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

.brand {
    display: inline-flex;
    gap: 12px;
    align-items: center;
}

.brand-mark {
    width: 48px;
    height: 48px;
    display: grid;
    place-items: center;
    border-radius: 16px;
    background: linear-gradient(135deg, #7b173f, #db7fa9);
    color: #fff;
    font-weight: 700;
    text-transform: uppercase;
    box-shadow: 0 18px 38px rgba(123, 23, 63, 0.25);
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
    padding: 38px 0 26px;
}

.hero::before {
    content: '';
    position: absolute;
    inset: 22px 0 auto;
    height: 580px;
    background: linear-gradient(120deg, #7b173f, #b98bd8, #ee95b4, #f7d5d1);
    background-size: 260% 260%;
    animation: gradientShift 14s ease infinite;
    opacity: 0.16;
    filter: blur(10px);
}

.hero-grid {
    position: relative;
    z-index: 1;
    display: grid;
    grid-template-columns: 1.1fr 0.9fr;
    gap: 22px;
    align-items: stretch;
}

.hero-copy,
.visual-card,
.soft-card,
.alive-card,
.footer-card {
    border: 1px solid rgba(122, 31, 70, 0.1);
    background: rgba(255, 255, 255, 0.76);
    box-shadow: 0 18px 45px rgba(123, 23, 63, 0.09);
    backdrop-filter: blur(16px);
}

.hero-copy {
    border-radius: 34px;
    padding: 42px;
}

.hero-kicker,
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

.hero-copy h1,
.section-head h2,
.alive-copy h2,
.footer-top h2 {
    margin: 18px 0 14px;
    color: #5f1433;
    line-height: 0.98;
}

.hero-copy h1 {
    font-size: clamp(2.5rem, 5vw, 4.8rem);
    min-height: 1.96em;
}

.hero-copy p,
.section-head,
.alive-copy p,
.step-card p,
.product-body p,
.footer-top p {
    color: #805a70;
    line-height: 1.72;
}

.hero-actions {
    display: flex;
    gap: 14px;
    flex-wrap: wrap;
    margin-top: 28px;
}

.btn {
    min-height: 50px;
    padding: 0 22px;
    border-radius: 999px;
    border: 0;
    cursor: pointer;
    transition: transform 0.25s ease, box-shadow 0.25s ease, background 0.25s ease;
}

.btn:hover {
    transform: translateY(-2px);
}

.btn-primary {
    background: linear-gradient(135deg, #7b173f, #e286ac);
    color: #fff;
    box-shadow: 0 16px 30px rgba(123, 23, 63, 0.22);
}

.btn-secondary {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    background: rgba(255, 255, 255, 0.9);
    color: #611635;
}

.btn-small {
    min-height: 44px;
    padding-inline: 18px;
}

.btn.is-bouncing {
    animation: buttonBounce 0.48s ease;
}

.slider-dots {
    display: flex;
    gap: 10px;
    margin-top: 28px;
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

.hero-visual {
    transform: translate3d(var(--mx), var(--my), 0);
    transition: transform 0.25s ease;
}

.visual-card {
    position: relative;
    min-height: 100%;
    border-radius: 34px;
    padding: 24px;
    overflow: hidden;
}

.visual-badge {
    display: inline-flex;
    padding: 9px 14px;
    border-radius: 999px;
    background: rgba(182, 143, 217, 0.18);
    color: #6e1e41;
    font-weight: 700;
}

.visual-stage {
    position: relative;
    min-height: 320px;
    margin-top: 18px;
    border-radius: 28px;
    background:
        radial-gradient(circle at 50% 10%, rgba(255, 255, 255, 0.88), transparent 38%),
        linear-gradient(145deg, rgba(246, 219, 228, 0.9), rgba(240, 231, 246, 0.9));
    overflow: hidden;
}

.floating-item {
    position: absolute;
    font-size: 2rem;
    animation: floatItem 5s ease-in-out infinite;
    filter: drop-shadow(0 12px 24px rgba(123, 23, 63, 0.18));
}

.gift-illustration {
    position: absolute;
    inset: 50% auto auto 50%;
    width: 190px;
    height: 190px;
    transform: translate(-50%, -44%);
}

.gift-box,
.gift-lid {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    border-radius: 28px;
    background: linear-gradient(145deg, #f4d7d2, #e98aac);
    box-shadow: 0 22px 42px rgba(123, 23, 63, 0.16);
}

.gift-box {
    bottom: 16px;
    width: 170px;
    height: 120px;
}

.gift-lid {
    top: 18px;
    width: 180px;
    height: 60px;
    transform: translateX(-50%) rotate(-4deg);
}

.gift-ribbon {
    position: absolute;
    inset: 14px auto 16px 50%;
    width: 24px;
    transform: translateX(-50%);
    background: linear-gradient(180deg, #7b173f, #b583d5);
    border-radius: 999px;
}

.visual-footer {
    display: grid;
    gap: 6px;
    margin-top: 18px;
    color: #79576a;
}

.section {
    padding: 26px 0;
}

.section-head h2,
.alive-copy h2,
.footer-top h2 {
    font-size: clamp(2rem, 3vw, 3rem);
}

.category-grid {
    display: grid;
    grid-template-columns: repeat(4, minmax(0, 1fr));
    gap: 14px;
    margin-top: 20px;
}

.soft-card {
    border-radius: 28px;
}

.category-card {
    display: flex;
    align-items: center;
    gap: 12px;
    min-height: 84px;
    padding: 20px;
    color: #69213f;
    font-weight: 600;
}

.category-icon {
    width: 34px;
    height: 34px;
    display: inline-grid;
    place-items: center;
    border-radius: 50%;
    background: rgba(182, 143, 217, 0.16);
}

.product-grid {
    display: grid;
    grid-template-columns: repeat(4, minmax(0, 1fr));
    gap: 18px;
    margin-top: 20px;
}

.hover-lift {
    transition: transform 0.28s ease, box-shadow 0.28s ease;
}

.hover-lift:hover {
    transform: translateY(-6px);
    box-shadow: 0 24px 45px rgba(123, 23, 63, 0.14);
}

.product-card {
    position: relative;
    overflow: hidden;
}

.favorite-btn {
    position: absolute;
    top: 16px;
    right: 16px;
    z-index: 2;
    width: 40px;
    height: 40px;
    border: 0;
    border-radius: 50%;
    background: rgba(255, 255, 255, 0.9);
    color: #8d5672;
    cursor: pointer;
    box-shadow: 0 10px 24px rgba(123, 23, 63, 0.12);
    transition: transform 0.25s ease, color 0.25s ease, background 0.25s ease;
}

.favorite-btn.active {
    color: #c23872;
    background: #fff1f5;
}

.favorite-btn:hover {
    transform: scale(1.06);
}

.product-media {
    padding: 22px 22px 12px;
}

.product-art {
    min-height: 220px;
    border-radius: 26px;
    background:
        radial-gradient(circle at top, rgba(255, 255, 255, 0.62), transparent 28%),
        linear-gradient(145deg, #7b173f, #b68fd9 58%, #e98aac);
    display: grid;
    place-items: center;
    overflow: hidden;
}

.product-art-inner {
    width: 72%;
    aspect-ratio: 1;
    display: grid;
    place-items: center;
    border-radius: 28px;
    background: rgba(255, 255, 255, 0.18);
    color: #fff;
    font-size: 3rem;
    border: 1px solid rgba(255, 255, 255, 0.26);
    transition: transform 0.45s ease;
}

.product-card:hover .product-art-inner {
    transform: scale(1.08);
}

.product-body {
    padding: 0 22px 22px;
}

.product-body h3,
.step-card h3,
.form-card h3 {
    margin: 0 0 10px;
    color: #631537;
}

.product-bottom {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 12px;
    margin-top: 18px;
}

.product-bottom strong {
    color: #631537;
    font-size: 1.2rem;
}

.alive-card {
    border-radius: 34px;
    padding: 28px;
    background:
        radial-gradient(circle at top right, rgba(255, 255, 255, 0.24), transparent 20%),
        linear-gradient(135deg, rgba(123, 23, 63, 0.96), rgba(182, 143, 217, 0.94));
    color: #fff;
}

.new-badge {
    display: inline-flex;
    align-items: center;
    padding: 10px 16px;
    border-radius: 999px;
    background: rgba(255, 255, 255, 0.14);
    border: 1px solid rgba(255, 255, 255, 0.2);
    font-weight: 800;
    letter-spacing: 0.04em;
    box-shadow:
        0 0 18px rgba(255, 198, 225, 0.5),
        0 0 34px rgba(255, 255, 255, 0.16);
    animation: badgeGlow 2.2s ease-in-out infinite;
}

.alive-copy p {
    max-width: 560px;
    color: rgba(255, 255, 255, 0.86);
}

.alive-pills {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    margin-top: 20px;
}

.alive-pill {
    padding: 10px 14px;
    border-radius: 999px;
    background: rgba(255, 255, 255, 0.14);
    border: 1px solid rgba(255, 255, 255, 0.14);
    color: #fff8fb;
}

.order-grid {
    display: grid;
    grid-template-columns: 1fr 0.88fr;
    gap: 20px;
    align-items: start;
}

.steps-grid {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 16px;
    margin-top: 20px;
}

.step-card {
    padding: 22px;
}

.step-index {
    display: inline-flex;
    margin-bottom: 12px;
    color: #a26483;
    font-weight: 800;
}

.form-card {
    padding: 26px;
}

.order-form {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 14px;
}

.order-form label {
    display: grid;
    gap: 8px;
    color: #6c3d57;
    font-size: 0.95rem;
}

.order-form label.full,
.order-form .full {
    grid-column: 1 / -1;
}

.order-form input,
.order-form textarea {
    width: 100%;
    border: 1px solid rgba(122, 31, 70, 0.12);
    border-radius: 18px;
    background: rgba(255, 255, 255, 0.92);
    padding: 14px 16px;
    color: #4b233c;
    outline: none;
    transition: border-color 0.25s ease, box-shadow 0.25s ease;
}

.order-form textarea {
    min-height: 120px;
    resize: vertical;
}

.order-form input:focus,
.order-form textarea:focus {
    border-color: rgba(123, 23, 63, 0.34);
    box-shadow: 0 0 0 4px rgba(233, 138, 172, 0.14);
}

.site-footer {
    padding: 0 0 32px;
}

.footer-card {
    border-radius: 34px;
    padding: 28px;
}

.footer-top {
    display: grid;
    grid-template-columns: 1fr auto;
    gap: 20px;
    align-items: end;
}

.contact-list,
.social-row {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
}

.contact-list a,
.social-row a {
    padding: 12px 16px;
    border-radius: 999px;
    background: rgba(248, 232, 237, 0.7);
    color: #671939;
    transition: transform 0.25s ease, background 0.25s ease;
}

.contact-list a:hover,
.social-row a:hover {
    transform: translateY(-2px);
    background: rgba(182, 143, 217, 0.16);
}

.social-row {
    margin-top: 18px;
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

@keyframes badgeGlow {
    0%,
    100% {
        box-shadow:
            0 0 18px rgba(255, 198, 225, 0.4),
            0 0 30px rgba(255, 255, 255, 0.12);
    }
    50% {
        box-shadow:
            0 0 26px rgba(255, 198, 225, 0.72),
            0 0 42px rgba(255, 255, 255, 0.2);
    }
}

@keyframes buttonBounce {
    0% {
        transform: scale(1);
    }
    30% {
        transform: scale(0.94);
    }
    60% {
        transform: scale(1.06);
    }
    100% {
        transform: scale(1);
    }
}

@media (max-width: 980px) {
    .header-row,
    .hero-grid,
    .order-grid,
    .footer-top {
        grid-template-columns: 1fr;
    }

    .nav {
        justify-content: flex-start;
    }

    .category-grid,
    .product-grid {
        grid-template-columns: repeat(2, minmax(0, 1fr));
    }
}

@media (max-width: 720px) {
    .hero-copy,
    .visual-card,
    .alive-card,
    .footer-card,
    .form-card {
        padding: 22px;
        border-radius: 26px;
    }

    .hero-copy h1 {
        min-height: auto;
    }

    .category-grid,
    .product-grid,
    .steps-grid,
    .order-form {
        grid-template-columns: 1fr;
    }

    .product-bottom,
    .hero-actions {
        flex-direction: column;
        align-items: stretch;
    }

    .btn,
    .btn-secondary {
        width: 100%;
    }

    .header-phone {
        justify-self: start;
    }
}
</style>
