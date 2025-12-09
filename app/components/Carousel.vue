<template>
    <div class="embla">
        <div class="embla__viewport" ref="viewportRef">
            <div class="embla__container">
                <div 
                class="embla__slide" 
                v-for="(img, i) in slides" 
                :key="i"
                >
                    <div class="embla__parallax">
                        <div class="embla__parallax__layer">
                            <img
                            class="embla__slide__img embla__parallax__img w-100"
                            :src="img"
                            alt="Your alt text"
                            />
                        </div>
                    </div>
                    
                </div>
            </div>
        </div>

        <!-- Prev / Next Buttons -->
        <button class="embla__prev" @click="scrollPrev">‹</button>
        <button class="embla__next" @click="scrollNext">›</button>

        <!-- Dot Navigation -->
        <div class="embla__dots">
            <button
                v-for="(dot, index) in dots"
                :key="index"
                class="embla__dot"
                :class="{ 'is-selected': selectedIndex === index }"
                @click="scrollTo(index)"
            ></button>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import EmblaCarousel from 'embla-carousel'

const slides = [
    'https://medicastore.com/images/slide/Hhh3A_12-09-2025.jpg',
    'https://medicastore.com/images/slide/smUY2_09-03-2024.webp',
    'https://medicastore.com/images/slide/PhstI_09-03-2024.webp',
    // '/images/4.jpg'
]

const viewportRef = ref(null)
const embla = ref(null)

const dots = ref([])
const selectedIndex = ref(0)

onMounted(() => {
    embla.value = EmblaCarousel(viewportRef.value, {
        loop: true,
        align: 'start'
    })

    // Setup dot count
    dots.value = embla.value.slideNodes().map(() => 0)

    // Update active dot
    const onSelect = () => {
        selectedIndex.value = embla.value.selectedScrollSnap()
    }

    embla.value.on('select', onSelect)
    onSelect()
})

const scrollPrev = () => {
    if (embla.value) embla.value.scrollPrev()
}

const scrollNext = () => {
    if (embla.value) embla.value.scrollNext()
}

const scrollTo = (index) => {
    if (embla.value) embla.value.scrollTo(index)
}
</script>

<style>
.embla {
    position: relative;
    margin: auto;
    overflow: hidden;
}
.embla__viewport {
    overflow: hidden;
    width: 100%;
}
.embla__container {
    display: flex;
    user-select: none;
}
.embla__slide {
    position: relative;
    min-width: 100%;
    padding: 10px;
}

/* Arrows */
.embla__prev,
.embla__next {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    width: 40px;
    height: 40px;
    border: none;
    background: rgba(0,0,0,0.5);
    color: white;
    font-size: 24px;
    border-radius: 50%;
    cursor: pointer;
}
.embla__prev { left: 10px; }
.embla__next { right: 10px; }

/* Dots */
.embla__dots {
    display: flex;
    justify-content: center;
    gap: 8px;
    margin-top: 10px;
}
.embla__dot {
    width: 10px;
    height: 10px;
    border-radius: 50%;
    background: #ccc;
    border: none;
    cursor: pointer;
}
.embla__dot.is-selected {
    background: #007bff;
}
</style>
