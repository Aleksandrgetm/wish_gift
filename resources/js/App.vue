<template>
    <div class="page-shell">
        <header class="site-header">
            <div class="container header-row">
                <a href="/" class="brand logo" @click.prevent="goToPage('/')">
                    <img :src="logoSrc" alt="wish gift" class="logo-img" />
                    <span class="brand-copy">
                        wish_gift
                        <small>Конфеты, сувениры, праздники</small>
                    </span>
                </a>

                <nav class="nav" aria-label="Основная навигация">
                    <a
                        v-for="item in navItems"
                        :key="item.path"
                        :href="item.path"
                        class="nav-link"
                        :class="{ active: isNavActive(item) }"
                        @click.prevent="handleNavClick(item)"
                    >
                        {{ item.label }}
                    </a>
                </nav>

                <div class="header-spacer" aria-hidden="true"></div>
            </div>
        </header>

        <div v-if="currentPage === 'home'" class="page-decor" aria-hidden="true">
            <span
                v-for="item in decorItems"
                :key="item.id"
                class="page-decor-item"
                :style="decorItemStyle(item)"
            >
                <span class="page-decor-glyph" :class="item.animation">
                    {{ item.emoji }}
                </span>
            </span>
        </div>

        <main>
            <template v-if="currentPage === 'home'">
                <section
                    id="top"
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

                    <div class="container hero-layout">
                        <div class="hero-copy">
                            <span class="eyebrow">Подарки и праздничные новинки</span>
                            <h1>Подарки, которые хочется дарить сразу</h1>
                            <p>
                                Собираем сладкие наборы, сувениры и персональные подарки с красивой
                                подачей, чтобы каждый заказ выглядел тёплым, аккуратным и особенным.
                            </p>

                            <div class="hero-actions">
                                <a href="/catalog" class="btn-primary" @click.prevent="goToPage('/catalog')">
                                    Перейти в каталог
                                </a>
                                <a href="/alive" class="btn-secondary" @click.prevent="goToPage('/alive')">
                                    Смотреть оживайки
                                </a>
                            </div>
                        </div>

                        <div class="hero-slider">
                            <div class="slide-frame" :style="heroParallaxStyle">
                                <div
                                    v-for="(slide, index) in slides"
                                    :key="slide.src"
                                    class="slide"
                                    :class="{ active: activeSlide === index }"
                                >
                                    <img
                                        v-if="!failedSlides[index]"
                                        :src="slide.src"
                                        alt="Wish Gift"
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
                                    class="slider-dot"
                                    :class="{ active: index === activeSlide }"
                                    type="button"
                                    :aria-label="`Слайд ${index + 1}`"
                                    @click="setSlide(index)"
                                />
                            </div>
                        </div>
                    </div>
                </section>

                <section id="new" class="section reveal" data-reveal>
                    <div class="container">
                        <div class="section-head">
                            <div>
                                <span class="section-label">Новинки</span>
                                <h2>Актуальные подарки для праздников и тёплых поводов</h2>
                            </div>
                            <a href="/catalog" class="btn-primary section-link" @click.prevent="goToPage('/catalog')">
                                Открыть каталог
                            </a>
                        </div>

                        <div class="product-grid preview-grid">
                            <article v-for="product in featuredProducts" :key="product.name" class="product-card">
                                <div class="product-image">
                                    <img :src="product.image" :alt="product.name" />
                                </div>
                                <div class="product-content">
                                    <h3>{{ product.name }}</h3>
                                    <p>{{ product.description }}</p>
                                    <div class="product-meta">
                                        <strong>{{ product.price }}</strong>
                                        <button type="button" class="btn-primary product-btn" @click="scrollToContacts">
                                            Заказать
                                        </button>
                                    </div>
                                </div>
                            </article>
                        </div>
                    </div>
                </section>

                <section class="section feature-band reveal" data-reveal>
                    <div class="container">
                        <div class="section-head">
                            <div>
                                <span class="section-label">Почему выбирают нас</span>
                                <h2>Подарки под событие, формат и настроение</h2>
                            </div>
                        </div>

                        <div class="feature-grid">
                            <article v-for="feature in homeFeatures" :key="feature.title" class="feature-card">
                                <div class="feature-icon">{{ feature.icon }}</div>
                                <h3>{{ feature.title }}</h3>
                                <p>{{ feature.text }}</p>
                            </article>
                        </div>
                    </div>
                </section>
            </template>

            <template v-else-if="currentPage === 'catalog'">
                <section class="page-hero reveal" data-reveal>
                    <div class="container page-hero-shell">
                        <div class="page-copy">
                            <span class="section-label">Каталог</span>
                            <h1>Подберите подарок под любой повод</h1>
                            <p>
                                Сладкие наборы, сувениры и готовые подарочные решения в мягком,
                                праздничном стиле. Все карточки можно использовать как основу для
                                вашего ассортимента.
                            </p>
                        </div>

                        <div class="page-hero-card">
                            <strong>8 товаров-заглушек</strong>
                            <span>Фотографии, описание, цена и кнопка заказа уже готовы.</span>
                        </div>
                    </div>
                </section>

                <section class="section reveal" data-reveal>
                    <div class="container">
                        <div class="product-grid catalog-grid">
                            <article v-for="product in catalogProducts" :key="product.name" class="product-card">
                                <div class="product-image">
                                    <img :src="product.image" :alt="product.name" />
                                </div>
                                <div class="product-content">
                                    <h3>{{ product.name }}</h3>
                                    <p>{{ product.description }}</p>
                                    <div class="product-meta">
                                        <strong>{{ product.price }}</strong>
                                        <button type="button" class="btn-primary product-btn" @click="scrollToContacts">
                                            Заказать
                                        </button>
                                    </div>
                                </div>
                            </article>
                        </div>
                    </div>
                </section>
            </template>

            <template v-else-if="currentPage === 'alive'">
                <section class="page-hero reveal" data-reveal>
                    <div class="container page-hero-shell alive-hero-shell">
                        <div class="page-copy">
                            <span class="section-label">Оживайки</span>
                            <h1>Сувениры, в которые можно встроить ваши фото и видео</h1>
                            <p>
                                Клиент отправляет фотографию и короткое видео, а мы аккуратно
                                встраиваем их в выбранный продукт, чтобы подарок стал по-настоящему
                                личным и запоминающимся.
                            </p>
                        </div>

                        <div class="alive-hero-visual">
                            <div class="alive-hero-badge">QR + фото + видео</div>
                            <img
                                v-if="!aliveHeroFailed"
                                :src="aliveHeroSrc"
                                alt="Wish Gift Alive"
                                @error="markAliveHeroError"
                            />
                            <div v-else class="slide-placeholder">
                                <div class="slide-placeholder-card slide-placeholder-card-main"></div>
                                <div class="slide-placeholder-card slide-placeholder-card-side"></div>
                                <div class="slide-placeholder-ribbon"></div>
                            </div>
                        </div>
                    </div>
                </section>

                <section class="section reveal" data-reveal>
                    <div class="container">
                        <div class="section-head">
                            <div>
                                <span class="section-label">Категории</span>
                                <h2>Что можно оживить</h2>
                            </div>
                        </div>

                        <div class="alive-categories">
                            <article
                                v-for="category in aliveCategories"
                                :key="category.title"
                                class="alive-category-card"
                            >
                                <div class="alive-category-icon">{{ category.icon }}</div>
                                <h3>{{ category.title }}</h3>
                                <p>{{ category.text }}</p>
                            </article>
                        </div>
                    </div>
                </section>

                <section class="section reveal" data-reveal>
                    <div class="container">
                        <div class="alive-layout">
                            <div class="alive-copy-card">
                                <span class="section-label">Как это работает</span>
                                <h2>Простой процесс от идеи до готового подарка</h2>
                                <p>
                                    Мы помогаем пройти весь путь спокойно: от выбора основы до
                                    финального макета и готового сувенира.
                                </p>
                            </div>

                            <div class="alive-steps-grid">
                                <article
                                    v-for="step in aliveProcess"
                                    :key="step.title"
                                    class="alive-step-card"
                                >
                                    <span class="alive-step-number">{{ step.number }}</span>
                                    <h3>{{ step.title }}</h3>
                                    <p>{{ step.text }}</p>
                                </article>
                            </div>
                        </div>
                    </div>
                </section>

                <section class="section reveal" data-reveal>
                    <div class="container">
                        <div class="feature-grid">
                            <article v-for="card in aliveSteps" :key="card.title" class="feature-card">
                                <div class="feature-icon">{{ card.icon }}</div>
                                <h3>{{ card.title }}</h3>
                                <p>{{ card.text }}</p>
                            </article>
                        </div>
                    </div>
                </section>
            </template>
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
                            Подарки из конфет, сувениры и персональные решения для праздников,
                            семейных событий и тёплых поздравлений.
                        </p>
                    </div>

                    <div class="footer-column">
                        <h3 class="footer-title">Навигация</h3>
                        <nav class="footer-links" aria-label="Footer navigation">
                            <a href="/" class="footer-link" @click.prevent="goToPage('/')">Главная</a>
                            <a href="/catalog" class="footer-link" @click.prevent="goToPage('/catalog')">Каталог</a>
                            <a href="/" class="footer-link" @click.prevent="scrollHomeToNew">Новинки</a>
                            <a href="/alive" class="footer-link" @click.prevent="goToPage('/alive')">Оживайки</a>
                            <a href="#contacts" class="footer-link" @click.prevent="scrollToContacts">Контакты</a>
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
                    </div>
                </div>

                <div class="footer-bottom">
                    <span>© 2026 wish_gift. Все права защищены.</span>
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
import { computed, nextTick, onBeforeUnmount, onMounted, ref, watch } from 'vue';

const logoSrc = '/images/logo.png';

const navItems = [
    { label: 'Главная', path: '/', page: 'home' },
    { label: 'Каталог', path: '/catalog', page: 'catalog' },
    { label: 'Новинки', path: '/', page: 'new', action: 'new' },
    { label: 'Оживайки', path: '/alive', page: 'alive' },
    { label: 'Контакты', path: '/', page: 'contacts', action: 'contacts' },
];

const galleryImagePaths = [
    '/images/chocolate 1.jpeg',
    '/images/chocolate 2.jpeg',
    '/images/chocolate 3.jpeg',
    '/images/chocolate 4.jpeg',
    '/images/chocolate 5.jpeg',
    '/images/chocolate 6.jpeg',
    '/images/chocolate 7.jpeg',
];

const slides = [
    ...galleryImagePaths.map((src, index) => ({
        src,
        kicker: index % 2 === 0 ? 'Подарочные наборы' : 'Wish Gift',
        caption:
            index % 2 === 0
                ? 'Сладкие композиции для тёплых поздравлений'
                : 'Реальные фотографии подарков и сувениров',
    })),
];

const floatingItems = [
    { emoji: '🍬', top: '11%', left: '6%', delay: '0s' },
    { emoji: '🎁', top: '16%', left: '92%', delay: '0.8s' },
    { emoji: '💝', top: '76%', left: '7%', delay: '1.4s' },
    { emoji: '✨', top: '63%', left: '91%', delay: '0.5s' },
];

const decorItems = [
    { id: 'd1', emoji: '🍬', top: '7%', left: '2.5%', size: 28, opacity: 0.14, color: '#d783b4', animation: 'float-y', speed: 0.08, delay: '0s' },
    { id: 'd2', emoji: '✨', top: '10%', right: '4%', size: 20, opacity: 0.12, color: '#8b2456', animation: 'pulse', speed: 0.05, delay: '0.8s' },
    { id: 'd3', emoji: '🎁', top: '15%', left: '10%', size: 34, opacity: 0.1, color: '#f2bfd7', animation: 'float-rotate', speed: 0.06, delay: '0.5s' },
    { id: 'd4', emoji: '💝', top: '18%', right: '11%', size: 42, opacity: 0.12, color: '#d783b4', animation: 'float-y', speed: 0.07, delay: '1.2s' },
    { id: 'd5', emoji: '🍬', top: '26%', left: '3%', size: 20, opacity: 0.11, color: '#8b2456', animation: 'pulse', speed: 0.04, delay: '1.1s' },
    { id: 'd6', emoji: '✨', top: '31%', right: '3.5%', size: 28, opacity: 0.16, color: '#f2bfd7', animation: 'float-rotate', speed: 0.05, delay: '0.3s' },
    { id: 'd7', emoji: '🎁', top: '39%', left: '7%', size: 34, opacity: 0.1, color: '#f8edf5', animation: 'float-y', speed: 0.08, delay: '1.6s' },
    { id: 'd8', emoji: '💝', top: '43%', right: '8%', size: 28, opacity: 0.14, color: '#d783b4', animation: 'pulse', speed: 0.06, delay: '0.6s' },
    { id: 'd9', emoji: '✨', top: '49%', left: '15%', size: 20, opacity: 0.1, color: '#8b2456', animation: 'float-y', speed: 0.05, delay: '1.9s' },
    { id: 'd10', emoji: '🍬', top: '52%', right: '14%', size: 42, opacity: 0.09, color: '#f2bfd7', animation: 'float-rotate', speed: 0.07, delay: '0.7s' },
    { id: 'd11', emoji: '🎁', top: '58%', left: '4%', size: 28, opacity: 0.13, color: '#d783b4', animation: 'float-y', speed: 0.08, delay: '1.4s' },
    { id: 'd12', emoji: '💝', top: '61%', right: '4.5%', size: 34, opacity: 0.15, color: '#8b2456', animation: 'pulse', speed: 0.05, delay: '0.4s' },
    { id: 'd13', emoji: '✨', top: '67%', left: '9%', size: 20, opacity: 0.08, color: '#f2bfd7', animation: 'float-rotate', speed: 0.06, delay: '1s' },
    { id: 'd14', emoji: '🍬', top: '72%', right: '10%', size: 28, opacity: 0.13, color: '#d783b4', animation: 'float-y', speed: 0.07, delay: '1.5s' },
    { id: 'd15', emoji: '🎁', top: '78%', left: '2%', size: 42, opacity: 0.1, color: '#f8edf5', animation: 'pulse', speed: 0.04, delay: '0.2s' },
    { id: 'd16', emoji: '💝', top: '82%', right: '2.5%', size: 34, opacity: 0.12, color: '#d783b4', animation: 'float-rotate', speed: 0.06, delay: '0.9s' },
    { id: 'd17', emoji: '✨', top: '87%', left: '12%', size: 20, opacity: 0.16, color: '#8b2456', animation: 'float-y', speed: 0.08, delay: '1.3s' },
    { id: 'd18', emoji: '🍬', top: '90%', right: '16%', size: 28, opacity: 0.11, color: '#f2bfd7', animation: 'pulse', speed: 0.05, delay: '1.7s' },
    { id: 'd19', emoji: '🎁', top: '93%', left: '5%', size: 34, opacity: 0.1, color: '#d783b4', animation: 'float-rotate', speed: 0.07, delay: '0.1s' },
    { id: 'd20', emoji: '💝', top: '95%', right: '5%', size: 42, opacity: 0.12, color: '#8b2456', animation: 'float-y', speed: 0.06, delay: '1.8s' },
];

const catalogProducts = [
    {
        name: 'Конфетный букет',
        description: 'Нежная сладкая композиция для дня рождения, юбилея или небольшого сюрприза.',
        price: 'от 29 €',
        image: galleryImagePaths[0],
    },
    {
        name: 'Подарочный бокс',
        description: 'Коробка с конфетами, декором и персональной открыткой в праздничном стиле.',
        price: 'от 34 €',
        image: galleryImagePaths[1],
    },
    {
        name: 'Сладкий набор Deluxe',
        description: 'Более крупный формат с акцентом на подачу и красивую упаковку.',
        price: 'от 46 €',
        image: galleryImagePaths[2],
    },
    {
        name: 'Именная кружка',
        description: 'Персональный сувенир с именем, фото или коротким пожеланием.',
        price: 'от 18 €',
        image: galleryImagePaths[3],
    },
    {
        name: 'Подарок для учителя',
        description: 'Аккуратный тематический набор для школы, выпускного или благодарности.',
        price: 'от 27 €',
        image: galleryImagePaths[4],
    },
    {
        name: 'Sweet Box Mini',
        description: 'Компактный подарок для коллег, гостей или небольших поздравлений.',
        price: 'от 16 €',
        image: galleryImagePaths[5],
    },
    {
        name: 'Romantic Box',
        description: 'Нежный подарок с конфетами и декоративными акцентами для особенного дня.',
        price: 'от 39 €',
        image: galleryImagePaths[6],
    },
    {
        name: 'Корпоративный подарок',
        description: 'Универсальная заготовка для брендинга, упаковки и праздничных заказов.',
        price: 'от 52 €',
        image: galleryImagePaths[0],
    },
];

const homeFeatures = [
    {
        icon: '🎀',
        title: 'Аккуратная упаковка',
        text: 'Собираем подарки так, чтобы они сразу выглядели готовыми к вручению.',
    },
    {
        icon: '💌',
        title: 'Персональный подход',
        text: 'Добавляем имена, открытки и детали, которые делают подарок более личным.',
    },
    {
        icon: '🌷',
        title: 'Под любой повод',
        text: 'Подбираем решения для праздников, семейных событий и небольших тёплых сюрпризов.',
    },
];

const aliveHeroCandidates = ['/images/alive.jpg', '/images/alive-1.jpg', galleryImagePaths[0]];

const aliveCategories = [
    {
        icon: '👕',
        title: 'Майка-оживайка',
        text: 'Футболка с персональной меткой, ведущей к вашему видео или истории.',
    },
    {
        icon: '🖼️',
        title: 'Фото-оживайка',
        text: 'Фотография, открытка или рамка, которую можно дополнить цифровым сюрпризом.',
    },
    {
        icon: '☕',
        title: 'Оживи кружку',
        text: 'Кружка с иллюстрацией или фото, которое дополняется вашим роликом.',
    },
    {
        icon: '🔑',
        title: 'Брелок',
        text: 'Небольшой сувенир с персональной ссылкой на памятное видео.',
    },
    {
        icon: '💌',
        title: 'Открытка',
        text: 'Поздравление, которое становится интерактивным и более запоминающимся.',
    },
    {
        icon: '🍫',
        title: 'Шоколад',
        text: 'Сладкий подарок с личным посланием, фото и цифровым дополнением.',
    },
    {
        icon: '🎵',
        title: 'Трек-пластинка',
        text: 'Декоративный подарок с любимой песней, историей и видео-посланием.',
    },
];

const aliveProcess = [
    {
        number: '1',
        title: 'Вы выбираете продукт',
        text: 'Подбираете формат подарка, который лучше всего подходит под ваш повод.',
    },
    {
        number: '2',
        title: 'Отправляете фото и видео',
        text: 'Передаёте материалы, которые хотите встроить в сувенир или упаковку.',
    },
    {
        number: '3',
        title: 'Мы создаём макет',
        text: 'Готовим визуальное решение и собираем итоговый продукт в аккуратном стиле.',
    },
    {
        number: '4',
        title: 'Вы получаете готовый подарок',
        text: 'На выходе получаете персональный сюрприз, который приятно дарить и хранить.',
    },
];

const aliveSteps = [
    { icon: '📷', title: 'Ваше фото', text: 'Отправляете фотографию, которую мы встраиваем в продукт.' },
    { icon: '🎥', title: 'Ваше видео', text: 'Добавляете короткий ролик для более живого и личного сюрприза.' },
    { icon: '🎁', title: 'Готовый подарок', text: 'Получаете современный персональный сувенир под ваш повод.' },
];

const featuredProducts = computed(() => catalogProducts.slice(0, 4));
const aliveHeroSrc = computed(() => aliveHeroCandidates[aliveHeroIndex.value] ?? galleryImagePaths[0]);

const currentPage = ref(resolvePage(window.location.pathname));
const activePage = ref(resolveActivePage(window.location.pathname));
const activeSlide = ref(0);
const failedSlides = ref({});
const aliveHeroIndex = ref(0);
const aliveHeroFailed = ref(false);
const decorScroll = ref(0);
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

watch(currentPage, async (page) => {
    document.title = pageTitles[page];
    syncAutoplay();

    await nextTick();
    observeRevealElements();
    window.scrollTo({ top: 0, behavior: 'smooth' });
});

function resolvePage(pathname) {
    if (pathname === '/catalog') {
        return 'catalog';
    }

    if (pathname === '/alive') {
        return 'alive';
    }

    return 'home';
}

function resolveActivePage(pathname) {
    if (pathname === '/catalog') {
        return 'catalog';
    }

    if (pathname === '/alive') {
        return 'alive';
    }

    return 'home';
}

const pageTitles = {
    home: 'wish_gift',
    catalog: 'Каталог | wish_gift',
    alive: 'Оживайки | wish_gift',
};

function isNavActive(item) {
    return activePage.value === item.page;
}

function handleNavClick(item) {
    if (item.action === 'new') {
        scrollHomeToNew();
        return;
    }

    if (item.action === 'contacts') {
        scrollToContacts();
        return;
    }

    goToPage(item.path);
}

function goToPage(path) {
    const nextPage = resolvePage(path);
    const nextActivePage = resolveActivePage(path);

    if (window.location.pathname !== path) {
        window.history.pushState({}, '', path);
    }

    currentPage.value = nextPage;
    activePage.value = nextActivePage;
}

async function scrollHomeToNew() {
    activePage.value = 'new';

    if (currentPage.value !== 'home') {
        goToPage('/');
        await nextTick();
        activePage.value = 'new';
    }

    window.requestAnimationFrame(() => {
        document.getElementById('new')?.scrollIntoView({ behavior: 'smooth', block: 'start' });
    });
}

function scrollToContacts() {
    activePage.value = 'contacts';
    document.getElementById('contacts')?.scrollIntoView({ behavior: 'smooth', block: 'start' });
}

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

    if (currentPage.value !== 'home') {
        autoplayId = null;
        return;
    }

    autoplayId = window.setInterval(() => {
        nextSlide();
    }, 4000);
}

function syncAutoplay() {
    if (currentPage.value === 'home') {
        restartAutoplay();
        return;
    }

    if (autoplayId) {
        window.clearInterval(autoplayId);
        autoplayId = null;
    }
}

function markSlideError(index) {
    failedSlides.value = {
        ...failedSlides.value,
        [index]: true,
    };
}

function markAliveHeroError() {
    if (aliveHeroIndex.value < aliveHeroCandidates.length - 1) {
        aliveHeroIndex.value += 1;
        return;
    }

    aliveHeroFailed.value = true;
}

function decorItemStyle(item) {
    return {
        top: item.top,
        left: item.left,
        right: item.right,
        '--decor-size': `${item.size}px`,
        '--decor-opacity': item.opacity,
        '--decor-color': item.color,
        '--decor-delay': item.delay,
        transform: `translate3d(0, ${decorScroll.value * item.speed}px, 0)`,
    };
}

function handleDecorScroll() {
    decorScroll.value = window.scrollY;
}

function onHeroMove(event) {
    if (currentPage.value !== 'home') {
        return;
    }

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

function createObserver() {
    observer?.disconnect();
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
}

function observeRevealElements() {
    if (!observer) {
        createObserver();
    }

    const items = document.querySelectorAll('[data-reveal]');
    items.forEach((item) => {
        item.classList.remove('is-visible');
        observer?.observe(item);
    });
}

function handlePopState() {
    currentPage.value = resolvePage(window.location.pathname);
    activePage.value = resolveActivePage(window.location.pathname);
}

onMounted(async () => {
    document.title = pageTitles[currentPage.value];
    createObserver();
    syncAutoplay();
    handleDecorScroll();

    await nextTick();
    observeRevealElements();

    window.addEventListener('popstate', handlePopState);
    window.addEventListener('scroll', handleDecorScroll, { passive: true });
});

onBeforeUnmount(() => {
    if (autoplayId) {
        window.clearInterval(autoplayId);
    }

    observer?.disconnect();
    window.removeEventListener('popstate', handlePopState);
    window.removeEventListener('scroll', handleDecorScroll);
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
    border: none;
    background: transparent;
    padding: 0;
    color: inherit;
    cursor: pointer;
    appearance: none;
}

.page-shell {
    position: relative;
    min-height: 100vh;
    overflow: hidden;
}

.page-decor {
    position: fixed;
    inset: 0;
    pointer-events: none;
    z-index: 0;
    overflow: hidden;
}

.page-decor-item {
    position: absolute;
    will-change: transform;
}

.page-decor-glyph {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    font-size: var(--decor-size);
    line-height: 1;
    opacity: var(--decor-opacity);
    color: var(--decor-color);
    filter: saturate(0.85);
    animation-delay: var(--decor-delay);
}

.page-decor-glyph.float-y {
    animation: floatY 8s ease-in-out infinite;
}

.page-decor-glyph.float-rotate {
    animation: floatRotate 12s linear infinite;
}

.page-decor-glyph.pulse {
    animation: pulse 6s ease infinite;
}

main,
.site-footer {
    position: relative;
    z-index: 2;
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
    background: rgba(255, 250, 252, 0.72);
    border-bottom: 1px solid rgba(122, 31, 70, 0.08);
    box-shadow: 0 8px 24px rgba(109, 31, 70, 0.04);
}

.header-row {
    display: grid;
    grid-template-columns: auto 1fr auto;
    align-items: center;
    gap: 24px;
}

.brand,
.logo {
    display: inline-flex;
    align-items: center;
    gap: 12px;
    border: 0;
    padding: 0;
    background: transparent;
    cursor: pointer;
    text-align: left;
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
    gap: 10px;
}

.header-spacer {
    width: 120px;
}

.nav-link {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    padding: 10px 14px;
    border-radius: 999px;
    color: #74455e;
    transition: transform 0.35s ease, background 0.35s ease, color 0.35s ease, box-shadow 0.35s ease;
}

.nav-link:hover,
.nav-link.active {
    background: rgba(255, 255, 255, 0.84);
    color: #611635;
    transform: translateY(-1px);
    box-shadow: 0 10px 24px rgba(109, 31, 70, 0.08);
}

.hero,
.page-hero {
    position: relative;
}

.hero {
    min-height: 610px;
    padding: 36px 0 28px;
    display: flex;
    align-items: center;
}

.hero::before,
.page-hero::before {
    content: '';
    position: absolute;
    inset: 0;
    pointer-events: none;
}

.hero::before {
    background:
        radial-gradient(circle at 15% 20%, rgba(215, 131, 180, 0.12), transparent 20%),
        radial-gradient(circle at 85% 16%, rgba(165, 60, 115, 0.08), transparent 18%);
}

.page-hero {
    padding: 54px 0 12px;
}

.page-hero::before {
    background:
        radial-gradient(circle at 12% 10%, rgba(255, 255, 255, 0.86), transparent 22%),
        radial-gradient(circle at 88% 18%, rgba(233, 138, 172, 0.12), transparent 24%);
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

.hero-layout,
.page-hero-shell {
    position: relative;
    z-index: 1;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 72px;
    align-items: center;
}

.hero-copy,
.page-copy {
    display: grid;
    gap: 14px;
}

.eyebrow,
.section-label {
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

.hero-copy h1,
.page-copy h1 {
    margin: 0;
    color: #6d1f46;
    font-size: clamp(2.3rem, 4vw, 4rem);
    line-height: 1;
}

.hero-copy p,
.page-copy p {
    margin: 0;
    max-width: 560px;
    color: #7f5670;
    font-size: 1rem;
    line-height: 1.75;
}

.hero-actions {
    display: flex;
    flex-wrap: wrap;
    gap: 14px;
    margin-top: 8px;
}

.btn-primary,
.btn-secondary,
.product-btn {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    text-decoration: none;
    border: none;
    cursor: pointer;
    transition: transform 0.35s ease, box-shadow 0.35s ease, background 0.35s ease, color 0.35s ease;
}

.btn-primary,
.product-btn {
    width: fit-content;
    min-height: 54px;
    padding: 0 28px;
    border-radius: 999px;
    background: linear-gradient(135deg, #6d1f46, #a53c73);
    color: #ffffff;
    font-weight: 700;
    box-shadow: 0 16px 36px rgba(109, 31, 70, 0.18);
}

.btn-secondary {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    min-height: 54px;
    padding: 0 24px;
    border-radius: 999px;
    background: rgba(255, 255, 255, 0.82);
    color: #6d1f46;
    box-shadow: 0 12px 28px rgba(109, 31, 70, 0.08);
}

.btn-primary:hover,
.btn-secondary:hover,
.product-btn:hover {
    transform: translateY(-3px);
}

.section-link {
    flex-shrink: 0;
}

.hero-slider {
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-bottom: 18px;
}

.slide-frame {
    position: relative;
    width: 100%;
    height: 520px;
    border-radius: 36px;
    overflow: hidden;
    background: #f8edf5;
    border: 1px solid rgba(215, 131, 180, 0.18);
    box-shadow: 0 30px 80px rgba(109, 31, 70, 0.12);
    transition: transform 0.45s ease;
}

.slide {
    position: absolute;
    inset: 0;
    opacity: 0;
    transform: scale(1.03);
    transition: opacity 0.7s ease, transform 0.7s ease;
}

.slide.active {
    opacity: 1;
    transform: scale(1);
    z-index: 2;
}

.slide img,
.alive-hero-visual img,
.product-image img {
    width: 100%;
    height: 100%;
    display: block;
    object-fit: cover;
    object-position: center;
    border-radius: 24px;
    transition: transform 0.35s ease;
}

.slide:hover img,
.alive-hero-visual:hover img,
.product-card:hover .product-image img {
    transform: scale(1.04);
}

.slide-placeholder {
    position: relative;
    width: 100%;
    height: 100%;
    background:
        linear-gradient(160deg, rgba(248, 237, 245, 0.95), rgba(255, 255, 255, 0.95)),
        radial-gradient(circle at top, rgba(165, 60, 115, 0.08), transparent 45%);
}

.slide-placeholder-card {
    position: absolute;
    border-radius: 28px;
    background: rgba(255, 255, 255, 0.88);
    box-shadow: 0 20px 40px rgba(109, 31, 70, 0.08);
}

.slide-placeholder-card-main {
    width: 54%;
    height: 62%;
    left: 15%;
    top: 18%;
}

.slide-placeholder-card-side {
    width: 30%;
    height: 36%;
    right: 12%;
    bottom: 16%;
}

.slide-placeholder-ribbon {
    position: absolute;
    width: 28%;
    height: 28%;
    right: 18%;
    top: 14%;
    border-radius: 50%;
    border: 14px solid rgba(165, 60, 115, 0.12);
}

.hero-slide-overlay {
    position: absolute;
    inset: auto 20px 20px 20px;
    padding: 18px 20px 20px;
    border-radius: 24px;
    background: linear-gradient(180deg, rgba(75, 35, 60, 0.05), rgba(75, 35, 60, 0.34));
    color: #ffffff;
}

.hero-slide-meta {
    display: grid;
    gap: 4px;
}

.hero-slide-meta span {
    font-size: 0.8rem;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    opacity: 0.82;
}

.hero-slide-meta strong {
    font-size: 1.1rem;
    line-height: 1.35;
}

.slider-dots {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 10px;
    margin-top: 24px;
}

.slider-dot {
    width: 10px;
    height: 10px;
    border-radius: 50%;
    background: rgba(109, 31, 70, 0.2);
    cursor: pointer;
    transition: all 0.3s ease;
}

.slider-dot.active {
    width: 34px;
    border-radius: 999px;
    background: #8b2456;
}

.slider-dot:hover {
    background: #a53c73;
}

.section {
    padding: 48px 0 30px;
}

.section-head {
    display: flex;
    align-items: end;
    justify-content: space-between;
    gap: 18px;
    margin-bottom: 26px;
}

.section-head h2,
.alive-copy-card h2 {
    margin: 10px 0 0;
    color: #6d1f46;
    font-size: clamp(1.9rem, 3vw, 2.8rem);
    line-height: 1.08;
}

.page-hero-card,
.alive-copy-card {
    padding: 28px;
    border-radius: 28px;
    background: linear-gradient(180deg, rgba(255, 255, 255, 0.96), rgba(248, 237, 245, 0.9));
    border: 1px solid rgba(215, 131, 180, 0.16);
    box-shadow: 0 18px 40px rgba(109, 31, 70, 0.08);
}

.page-hero-card {
    display: grid;
    gap: 10px;
    color: #7f5670;
}

.page-hero-card strong {
    color: #6d1f46;
    font-size: 1.1rem;
}

.product-grid,
.feature-grid,
.alive-categories,
.alive-steps-grid {
    display: grid;
    gap: 18px;
}

.preview-grid {
    grid-template-columns: repeat(4, minmax(0, 1fr));
}

.catalog-grid {
    grid-template-columns: repeat(4, minmax(0, 1fr));
}

.product-card,
.feature-card,
.alive-category-card,
.alive-step-card {
    background: rgba(255, 255, 255, 0.92);
    border: 1px solid rgba(215, 131, 180, 0.16);
    box-shadow: 0 16px 34px rgba(109, 31, 70, 0.06);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.product-card:hover,
.feature-card:hover,
.alive-category-card:hover,
.alive-step-card:hover {
    transform: translateY(-6px);
    box-shadow: 0 22px 42px rgba(109, 31, 70, 0.1);
}

.product-card {
    display: flex;
    flex-direction: column;
    overflow: hidden;
    border-radius: 24px;
    padding: 20px;
    gap: 18px;
}

.product-image {
    height: 220px;
    background: rgba(248, 237, 245, 0.8);
    border-radius: 24px;
    overflow: hidden;
}

.product-content {
    display: grid;
    gap: 14px;
    padding: 0;
}

.product-content h3,
.feature-card h3,
.alive-category-card h3,
.alive-step-card h3 {
    margin: 0;
    color: #6d1f46;
}

.product-content p,
.feature-card p,
.alive-category-card p,
.alive-step-card p,
.alive-copy-card p {
    margin: 0;
    color: #7b6a76;
    line-height: 1.7;
}

.product-meta {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 12px;
}

.product-meta strong {
    color: #6d1f46;
    font-size: 1.05rem;
}

.product-btn {
    min-height: 46px;
    padding: 0 20px;
}

.feature-band {
    padding-top: 28px;
}

.feature-grid {
    grid-template-columns: repeat(3, minmax(0, 1fr));
}

.feature-card {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 14px;
    min-height: 180px;
    padding: 28px 24px;
    border-radius: 20px;
    text-align: center;
}

.feature-icon,
.alive-category-icon {
    width: 56px;
    height: 56px;
    border-radius: 50%;
    background: #f8edf5;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 24px;
    box-shadow: 0 8px 20px rgba(109, 31, 70, 0.08);
    transition: transform 0.3s ease;
}

.feature-card:hover .feature-icon,
.alive-category-card:hover .alive-category-icon {
    transform: scale(1.08);
}

.feature-card p {
    max-width: 290px;
}

.alive-hero-shell {
    align-items: stretch;
}

.alive-hero-visual {
    position: relative;
    min-height: 360px;
    overflow: hidden;
    border-radius: 24px;
    border: 1px solid rgba(215, 131, 180, 0.16);
    box-shadow: 0 24px 56px rgba(109, 31, 70, 0.1);
}

.alive-hero-badge {
    position: absolute;
    top: 18px;
    left: 18px;
    z-index: 1;
    padding: 10px 16px;
    border-radius: 999px;
    background: rgba(255, 255, 255, 0.9);
    color: #7b173f;
    font-weight: 700;
    box-shadow: 0 12px 24px rgba(109, 31, 70, 0.1);
}

.alive-categories {
    grid-template-columns: repeat(4, minmax(0, 1fr));
}

.alive-category-card {
    display: grid;
    gap: 14px;
    align-content: start;
    padding: 24px;
    border-radius: 24px;
}

.alive-layout {
    display: grid;
    grid-template-columns: minmax(280px, 0.9fr) minmax(0, 1.1fr);
    gap: 20px;
    align-items: start;
}

.alive-steps-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
}

.alive-step-card {
    position: relative;
    padding: 24px;
    border-radius: 24px;
}

.alive-step-number {
    display: inline-flex;
    width: 42px;
    height: 42px;
    align-items: center;
    justify-content: center;
    margin-bottom: 16px;
    border-radius: 50%;
    background: linear-gradient(135deg, #6d1f46, #a53c73);
    color: #ffffff;
    font-weight: 700;
    box-shadow: 0 10px 22px rgba(109, 31, 70, 0.18);
}

.site-footer {
    width: 100%;
    margin-top: 26px;
    padding: 74px 0 30px;
    background: linear-gradient(135deg, #fff8fb, #f8edf5);
    border-top: 1px solid rgba(165, 60, 115, 0.15);
}

.footer-wrap {
    max-width: 1200px;
}

.footer-grid {
    display: grid;
    grid-template-columns: 1.1fr 0.9fr 0.9fr;
    gap: 40px;
}

.footer-logo-line {
    display: inline-flex;
    align-items: center;
    gap: 12px;
}

.footer-logo-img {
    width: 48px;
    height: 48px;
    border-radius: 14px;
    object-fit: contain;
}

.footer-brand-name,
.footer-title {
    color: #621938;
}

.footer-description {
    max-width: 360px;
    margin: 16px 0 0;
    color: #7f5670;
    line-height: 1.75;
}

.footer-title {
    margin: 0 0 16px;
    font-size: 1.05rem;
}

.footer-links,
.footer-contacts {
    display: grid;
    gap: 12px;
}

.footer-link {
    color: #7f5670;
    text-decoration: none;
    transition: color 0.25s ease, transform 0.25s ease;
}

.footer-link:hover {
    color: #611635;
    transform: translateX(2px);
}

.footer-contact-link {
    display: inline-flex;
    align-items: center;
    gap: 10px;
    color: #7f5670;
}

.footer-contact-icon {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 32px;
    height: 32px;
    border-radius: 50%;
    background: rgba(255, 255, 255, 0.9);
}

.footer-bottom {
    margin-top: 34px;
    padding-top: 20px;
    border-top: 1px solid rgba(165, 60, 115, 0.12);
    display: flex;
    align-items: center;
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

@keyframes floatY {
    0%,
    100% {
        transform: translateY(0);
    }
    50% {
        transform: translateY(-18px);
    }
}

@keyframes floatRotate {
    0% {
        transform: rotate(0deg);
    }
    100% {
        transform: rotate(360deg);
    }
}

@keyframes pulse {
    0%,
    100% {
        opacity: 0.08;
        transform: scale(1);
    }
    50% {
        opacity: 0.18;
        transform: scale(1.15);
    }
}

@media (max-width: 1100px) {
    .preview-grid,
    .catalog-grid,
    .alive-categories {
        grid-template-columns: repeat(2, minmax(0, 1fr));
    }
}

@media (max-width: 980px) {
    .header-row,
    .hero-layout,
    .page-hero-shell,
    .alive-layout,
    .footer-grid {
        grid-template-columns: 1fr;
    }

    .header-row {
        grid-template-columns: 1fr;
        justify-items: start;
    }

    .nav {
        justify-content: flex-start;
    }

    .header-spacer {
        display: none;
    }

    .feature-grid,
    .alive-steps-grid {
        grid-template-columns: 1fr 1fr;
    }
}

@media (max-width: 720px) {
    .hero {
        min-height: auto;
        padding-top: 28px;
    }

    .slide-frame {
        height: 400px;
        border-radius: 28px;
    }

    .page-hero,
    .section,
    .site-footer {
        padding-top: 28px;
    }

    .preview-grid,
    .catalog-grid,
    .feature-grid,
    .alive-categories,
    .alive-steps-grid {
        grid-template-columns: 1fr;
    }

    .section-head,
    .product-meta,
    .footer-bottom,
    .footer-bottom-links {
        flex-direction: column;
        align-items: flex-start;
    }

    .product-meta {
        gap: 14px;
    }

    .alive-hero-visual {
        min-height: 280px;
    }
}
</style>
