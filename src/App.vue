<script setup>
const mapsUrl = 'https://maps.app.goo.gl/WvyHEy2XpubUGMwd8'

const schedule = [
  { time: '16H', text: 'DOLAZAK GOSTIJU U RESTORAN' },
  { time: '17H', text: 'GRADJANSKO VENČANJE U RESTORANU' },
  { time: '20H', text: 'SEČENJE TORTE POD ZVEZDANIM NEBOM I NASTAVAK ŽURKE' },
  { time: '00H', text: 'POSLEDNJI PLES VEČERI' },
]
</script>

<template>
  <div class="invite mx-auto w-full max-w-md overflow-x-hidden text-ink">
    <!-- HERO -->
    <section class="hero relative flex min-h-[100svh] flex-col px-5 pb-10 pt-10">
      <h1
        class="font-script relative mx-auto flex flex-col items-center gap-1 text-center text-[clamp(2.75rem,6rem,10rem)] leading-[0.5]">
        <span class="whitespace-nowrap">Anja &amp; Luka</span>
        <span class="text-[0.92em]">vas pozivaju na</span>
        <span class="relative pb-3 text-[0.92em]">
          venčanje
          <img src="/assets/underline.png" alt=""
            class="pointer-events-none absolute -bottom-1 left-1/2 w-[min(280px,85vw)] -translate-x-1/2"
            aria-hidden="true" />
        </span>
      </h1>

      <div class="mt-10 flex flex-1 items-center justify-center">
        <div
          class="polaroid w-[min(72vw,280px)] rotate-[7deg] border-[10px] border-b-[22px] border-white bg-white shadow-[0_12px_28px_rgba(0,0,0,0.22)]">
          <img src="/assets/us-at-sea.jpg" alt="Anja i Luka"
            class="aspect-square w-full object-cover object-center grayscale" />
        </div>
      </div>

      <div
        class="font-typewriter mt-10 flex flex-col items-center gap-3 text-center text-[0.95rem] font-bold tracking-wide uppercase">
        <p>April, 17.2027.</p>
        <div class="leading-snug">
          <p>Restoran Verde,</p>
          <p>Zrenjaninski put 106A</p>
        </div>
      </div>
    </section>

    <!-- CELEBRATE COLLAGE -->
    <section class="collage relative overflow-hidden py-16">
      <div class="absolute inset-0 bg-cover bg-center grayscale" style="background-image: url('/assets/night-sky.jpg')"
        aria-hidden="true" />
      <div class="absolute inset-0 bg-black/25" aria-hidden="true" />

      <div class="relative mx-auto h-[420px] w-[min(92vw,360px)]">
        <!-- Envelope -->
        <img src="/assets/envelope.png" alt=""
          class="absolute bottom-4 left-1/2 w-[78%] -translate-x-1/2 drop-shadow-lg" aria-hidden="true" />

        <!-- Couple polaroid -->
        <div
          class="absolute top-8 left-2 w-[48%] -rotate-[14deg] border-[8px] border-white bg-white shadow-[0_10px_24px_rgba(0,0,0,0.35)]">
          <img src="/assets/couple-collage.jpg" alt="" class="aspect-[4/5] w-full object-cover grayscale"
            aria-hidden="true" />
        </div>

        <!-- Lace card -->
        <div
          class="lace-card absolute top-14 right-0 flex w-[58%] rotate-[12deg] flex-col items-center justify-center gap-3 px-5 py-9 text-center shadow-[0_10px_24px_rgba(0,0,0,0.28)]">
          <img src="/assets/lace.png" alt="" class="lace-edge lace-top" aria-hidden="true" />
          <img src="/assets/lace.png" alt="" class="lace-edge lace-bottom" aria-hidden="true" />
          <img src="/assets/lace.png" alt="" class="lace-edge lace-side lace-left" aria-hidden="true" />
          <img src="/assets/lace.png" alt="" class="lace-edge lace-side lace-right" aria-hidden="true" />
          <p class="font-typewriter relative z-10 text-[0.65rem] font-bold tracking-wide uppercase">
            April, 17.2027.
          </p>
          <p class="font-script relative z-10 text-[2rem] leading-[0.8]">
            Hajde da proslavimo zajedno!!!
          </p>
          <p class="font-typewriter relative z-10 text-[0.7rem] font-bold tracking-wide uppercase">
            Anja &amp; Luka
          </p>
        </div>
      </div>
    </section>

    <!-- PROSLAVA / VENUE -->
    <section class="px-5 py-14">
      <div class="relative mx-auto mb-10 max-w-[20rem] text-center">
        <h2 class="font-script text-[clamp(2.4rem,3.5rem,10rem)] leading-[1.05]">
          Proslava pod zvezdama
        </h2>
        <img src="/assets/scribble.png" alt="" class="mx-auto mt-1 w-[85%]" aria-hidden="true" />
      </div>

      <div class="flex flex-col items-center gap-6">
        <img src="/assets/venue.jpg" alt="Restoran Verde"
          class="w-full max-w-[280px] object-cover grayscale shadow-[0_8px_20px_rgba(0,0,0,0.15)]" />

        <p class="font-typewriter max-w-[22rem] text-center text-[0.95rem] leading-relaxed">
          Restoran Verde se nalazi na adresi Zrenjaninski put 106A u Borči. Prijem zvanica će biti sa
          početkom u 16h. Kliknite dugme za tačnu lokaciju:
        </p>

        <a :href="mapsUrl" target="_blank" rel="noopener noreferrer"
          class="inline-flex min-h-14 min-w-14 items-center justify-center transition-transform active:scale-95"
          aria-label="Otvori lokaciju na mapi">
          <img src="/assets/map-icon.png" alt="" class="h-16 w-auto" aria-hidden="true" />
        </a>
      </div>
    </section>

    <!-- SCHEDULE -->
    <section class="px-5 pb-14 pt-4">
      <div class="relative mx-auto mb-10 max-w-[18rem] text-center">
        <h2 class="font-script text-[clamp(2.4rem,3.5rem,10rem)] leading-[1.05]">Raspored dana</h2>
        <img src="/assets/scribble.png" alt="" class="mx-auto mt-1 w-[90%]" aria-hidden="true" />
      </div>

      <ul class="font-typewriter mx-auto flex max-w-[22rem] flex-col gap-6">
        <li v-for="item in schedule" :key="item.time" class="flex gap-3 text-[0.95rem] leading-snug">
          <span class="shrink-0 font-bold">{{ item.time }}</span>
          <span class="uppercase">{{ item.text }}</span>
        </li>
      </ul>
    </section>

    <!-- DRAGI NAŠI -->
    <section class="px-5 py-10">
      <div class="relative mx-auto mb-8 max-w-[14rem] text-center">
        <h2 class="font-script text-[clamp(2.4rem,3.5rem,10rem)] leading-[1.05]">Dragi naši</h2>
        <img src="/assets/pen-line.svg" alt="" class="mx-auto mt-2 w-[70%]" aria-hidden="true" />
      </div>

      <p class="font-typewriter mx-auto max-w-[22rem] text-center text-[0.95rem] leading-relaxed">
        Ovaj poseban dan želimo da proslavimo u društvu odraslih
        <strong>(18+)</strong>, kako bismo se svi dobro proveli i nazdravili. Hvala vam što poštujete
        našu želju i što ćete ovaj dan učiniti još lepšim. Radujemo se da vas vidimo.
      </p>
    </section>

    <!-- RSVP PLACEHOLDER -->
    <section class="px-5 pb-16 pt-8">
      <img src="/assets/anja-i-luka.jpg" alt="Anja i Luka" class="letterbox w-full object-contain grayscale" />

      <div class="relative mx-auto mt-12 mb-6 max-w-[20rem] text-center">
        <p class="font-script text-[clamp(1.7rem,4rem,10rem)] leading-[0.7]">
          Molimo Vas da nam potvrdite dolazak do 31. Marta
        </p>
        <img src="/assets/scribble.png" alt="" class="mx-auto mt-2 w-[95%]" aria-hidden="true" />
      </div>

      <p class="font-typewriter mx-auto mb-6 max-w-[20rem] text-center text-[0.95rem] leading-relaxed">
        Dolazak nam možete potvrditi putem dugmeta ispod:
      </p>

      <!-- Placeholder until Sheet Monkey form is wired -->
      <div class="flex justify-center" aria-hidden="true">
        <img src="/assets/rsvp-icon.png" alt="" class="h-16 w-auto opacity-90" />
      </div>
    </section>
  </div>
</template>

<style scoped>
.hero {
  background-image: url('/assets/paper-texture.jpg');
  background-size: cover;
  background-position: center;
}

.lace-card {
  position: absolute;
  background-color: #f5f0e6;
  background-image: url('/assets/card-paper.png');
  background-size: cover;
  background-position: center;
}

.lace-edge {
  position: absolute;
  pointer-events: none;
  z-index: 2;
}

.lace-top,
.lace-bottom {
  left: 0%;
  width: 100%;
  height: 20px;
  object-fit: fill;
}

.lace-top {
  top: -20px;
}

.lace-bottom {
  bottom: -20px;
  transform: scaleY(-1);
}

.lace-side {
  top: 50%;
  width: 90%;
  height: 20px;
  object-fit: fill;
}

.lace-left {
  left: -5px;
  transform: translate(-50%, -50%) rotate(-90deg);
}

.lace-right {
  right: -5px;
  left: auto;
  transform: translate(50%, -50%) rotate(90deg);
}

.letterbox {
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.12);
}
</style>
