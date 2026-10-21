<script setup>
import { ref } from 'vue'

const mapsUrl = 'https://maps.google.com/?cid=17048520087372756887'
const formUrl = 'https://api.sheetmonkey.io/form/6tBMVmvJ24ZcXDCj31yvke'
const nightSkyUrl = `${import.meta.env.BASE_URL}assets/night-sky.jpg`

const schedule = [
  { time: '16H', text: 'DOLAZAK GOSTIJU U RESTORAN' },
  { time: '17H', text: 'GRADJANSKO VENČANJE U RESTORANU' },
  { time: '20H', text: 'SEČENJE TORTE POD ZVEZDANIM NEBOM I NASTAVAK ŽURKE' },
  { time: '00H', text: 'POSLEDNJI PLES VEČERI' },
]

const dialogRef = ref(null)
const step = ref('form')
const submitting = ref(false)
const formData = ref({
  ime: '',
  prezime: '',
  adult: false,
})

const calendarEvent = {
  title: 'Anja i Luka svadba',
  details: `16h Anja i Luka svadba u Verde restoranu\n${mapsUrl}`,
  location: 'Verde venčanja na otvorenom, Zrenjaninski put 106A, Beograd 11000',
  start: '20270417T160000',
  end: '20270418T000000',
  startUtc: '20270417T140000Z',
  endUtc: '20270417T220000Z',
}

const googleCalendarUrl = [
  'https://calendar.google.com/calendar/render?action=TEMPLATE',
  `text=${encodeURIComponent(calendarEvent.title)}`,
  `dates=${calendarEvent.start}/${calendarEvent.end}`,
  'ctz=Europe/Belgrade',
  `location=${encodeURIComponent(calendarEvent.location)}`,
  `details=${encodeURIComponent(calendarEvent.details)}`,
].join('&')

const openModal = () => {
  step.value = 'form'
  dialogRef.value?.showModal()
}

const closeModal = () => {
  dialogRef.value?.close()
}

const resetDialog = () => {
  step.value = 'form'
  submitting.value = false
}

const onSubmit = async () => {
  if (!formData.value.adult || submitting.value) return

  submitting.value = true
  const body = new FormData()
  const confirmedOn = new Intl.DateTimeFormat('en-GB', {
    day: '2-digit',
    month: '2-digit',
    year: 'numeric',
    timeZone: 'Europe/Belgrade',
  })
    .format(new Date())
    .replaceAll('/', '.')

  body.append('Ime', formData.value.ime.trim())
  body.append('Prezime', formData.value.prezime.trim())
  body.append('Potvrdjeno', confirmedOn)

  try {
    await fetch(formUrl, { method: 'POST', body, mode: 'no-cors' })
    step.value = 'calendar'
  } finally {
    submitting.value = false
  }
}

const downloadIcs = () => {
  const ics = [
    'BEGIN:VCALENDAR',
    'VERSION:2.0',
    'PRODID:-//Anja i Luka//Svadba//SR',
    'CALSCALE:GREGORIAN',
    'METHOD:PUBLISH',
    'BEGIN:VEVENT',
    'UID:anja-luka-svadba-20270417@invite',
    `DTSTAMP:${calendarEvent.startUtc}`,
    `DTSTART:${calendarEvent.startUtc}`,
    `DTEND:${calendarEvent.endUtc}`,
    `SUMMARY:${calendarEvent.title}`,
    `DESCRIPTION:${calendarEvent.details.replaceAll('\n', '\\n')}`,
    `LOCATION:${calendarEvent.location.replaceAll(',', '\\,')}`,
    `URL:${mapsUrl}`,
    'END:VEVENT',
    'END:VCALENDAR',
  ].join('\r\n')

  const blob = new Blob([ics], { type: 'text/calendar;charset=utf-8' })
  const url = URL.createObjectURL(blob)
  const link = document.createElement('a')
  link.href = url
  link.download = 'anja-i-luka-svadba.ics'
  link.click()
  URL.revokeObjectURL(url)
}

const addToCalendar = () => {
  downloadIcs()
  window.open(googleCalendarUrl, '_blank', 'noopener,noreferrer')
  closeModal()
}
</script>

<template>
  <div class="invite mx-auto w-full max-w-md overflow-x-hidden text-ink">
    <!-- HERO -->
    <section class="hero relative flex min-h-[100svh] flex-col px-5 pb-10 pt-10">
      <h1
        class="font-script relative mx-auto flex flex-col items-center gap-1 text-center text-[clamp(2.75rem,7rem,10rem)] tracking-tighter leading-[0.6]">
        <span class="whitespace-nowrap">Anja &amp; Luka</span>
        <span class="text-[0.92em]">vas pozivaju na</span>
        <span class="relative pb-3 text-[0.92em]">
          venčanje
          <img src="/assets/underline.png" alt=""
            class="pointer-events-none absolute -bottom-1 left-1/2 w-[min(280px,85vw)] -translate-x-1/2"
            aria-hidden="true" />
        </span>
      </h1>

      <img src="/assets/wedding-rings.svg" alt=""
        class="pointer-events-none relative z-10 mx-auto mt-7 w-[4.75rem] rotate-[-8deg] opacity-80"
        aria-hidden="true" />

      <div class="mt-6 flex flex-1 items-center justify-center">
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
      <div
        class="absolute inset-0 bg-cover bg-center grayscale"
        :style="{ backgroundImage: `url('${nightSkyUrl}')` }"
        aria-hidden="true"
      />
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
          class="lace-card absolute top-55 right-0 flex w-[58%] rotate-[12deg] flex-col items-center justify-center gap-3 px-5 py-9 text-center shadow-[0_10px_24px_rgba(0,0,0,0.28)]">
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
        <h2 class="font-script text-[clamp(2.4rem,4rem,10rem)] leading-[1.05] tracking-tighter">
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
    <section class="px-5 pb-14">
      <div class="relative mx-auto mb-10 max-w-[18rem] text-center">
        <h2 class="font-script text-[clamp(2.4rem,5rem,10rem)] leading-[1.05] tracking-tighter">Raspored dana</h2>
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
    <section class="px-5 pb-10">
      <div class="relative mx-auto mb-8 max-w-[14rem] text-center">
        <h2 class="font-script text-[clamp(2.4rem,5rem,10rem)] leading-[1.05] tracking-tighter">Dragi naši</h2>
        <img src="/assets/pen-line.svg" alt="" class="mx-auto mt-2 w-[70%]" aria-hidden="true" />
      </div>

      <p class="font-typewriter mx-auto max-w-[22rem] text-center text-[0.95rem] leading-relaxed">
        Ovaj poseban dan želimo da proslavimo u društvu odraslih
        <strong>(18+)</strong>, kako bismo se svi dobro proveli i nazdravili. Hvala vam što poštujete
        našu želju i što ćete ovaj dan učiniti još lepšim. Radujemo se da vas vidimo.
      </p>
    </section>

    <!-- RSVP -->
    <section class="px-5 pb-16 pt-8">
      <img src="/assets/anja-i-luka.jpg" alt="Anja i Luka" class="letterbox w-full object-contain grayscale" />

      <div class="relative mx-auto mt-12 mb-6 max-w-[20rem] text-center">
        <p class="font-script text-[clamp(1.7rem,5rem,10rem)] leading-[1] tracking-tighter">
          Molimo Vas da nam potvrdite dolazak do 31. Marta
        </p>
        <img src="/assets/scribble.png" alt="" class="mx-auto mt-2 w-[95%]" aria-hidden="true" />
      </div>

      <p class="font-typewriter mx-auto mb-6 max-w-[20rem] text-center text-[0.95rem] leading-relaxed">
        Dolazak nam možete potvrditi putem koverte ispod:
      </p>

      <div class="flex justify-center">
        <button type="button" class="transition-transform active:scale-95" aria-label="Otvori potvrdu dolaska"
          @click="openModal">
          <img src="/assets/rsvp-icon.png" alt="" class="h-16 w-auto opacity-90" />
        </button>
      </div>
    </section>
  </div>

  <dialog ref="dialogRef" class="rsvp-dialog" @close="resetDialog">
    <button type="button" class="rsvp-close" aria-label="Zatvori" @click="closeModal">
      ×
    </button>

    <div v-if="step === 'form'" class="flex flex-col items-center text-center">
      <h2 class="font-script text-[clamp(2.4rem,3.4rem,4rem)] leading-[0.9] tracking-tighter">
        Potvrda dolaska
      </h2>
      <img src="/assets/scribble.png" alt="" class="mt-1 mb-8 w-[70%]" aria-hidden="true" />

      <form class="flex w-full flex-col gap-6" @submit.prevent="onSubmit">
        <label class="field">
          <span>Ime</span>
          <input v-model="formData.ime" type="text" name="Ime" autocomplete="given-name" required maxlength="80" />
        </label>

        <label class="field">
          <span>Prezime</span>
          <input v-model="formData.prezime" type="text" name="Prezime" autocomplete="family-name" required
            maxlength="80" />
        </label>

        <label class="age-check">
          <input v-model="formData.adult" type="checkbox" required />
          <span>Potvrđujem da imam 18 ili više godina</span>
        </label>

        <div class="mt-2 flex flex-col items-center gap-3">
          <button type="submit" class="rsvp-btn" :disabled="!formData.adult || submitting">
            {{ submitting ? 'Šaljemo…' : 'Potvrdi dolazak' }}
          </button>
          <button type="button" class="rsvp-link" @click="closeModal">Otkaži</button>
        </div>
      </form>
    </div>

    <div v-else class="flex flex-col items-center text-center">
      <h2 class="font-script text-[clamp(2.4rem,3.4rem,4rem)] leading-[0.9] tracking-tighter">
        Hvala vam
      </h2>
      <img src="/assets/scribble.png" alt="" class="mt-1 mb-6 w-[70%]" aria-hidden="true" />

      <p class="font-typewriter mb-5 max-w-[18rem] text-[0.95rem] leading-relaxed">
        Želite li da dodate datum u kalendar?
      </p>

      <div class="calendar-card mb-7">
        <p class="font-typewriter text-[0.72rem] font-bold tracking-[0.16em] uppercase">17. april 2027.</p>
        <p class="font-script mt-2 text-[1.85rem] leading-[0.9]">16h Anja i Luka svadba</p>
        <p class="font-typewriter mt-3 text-[0.85rem]">u Verde restoranu</p>
      </div>

      <div class="flex w-full flex-col items-center gap-3">
        <button type="button" class="rsvp-btn" @click="addToCalendar">Dodaj u kalendar</button>
        <button type="button" class="rsvp-link" @click="closeModal">Ne, hvala</button>
      </div>
    </div>
  </dialog>
</template>

<style scoped>
button {
  background: none;
  border: none;
  cursor: pointer;
  color: inherit;
}

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

.rsvp-dialog {
  width: min(92vw, 26rem);
  max-width: 26rem;
  max-height: min(90vh, 40rem);
  overflow-y: auto;
  margin: auto;
  padding: 2.6rem 1.6rem 1.85rem;
  border: none;
  color: var(--color-ink);
  background-color: #f3eee4;
  background-image: url('/assets/paper-texture.jpg');
  background-size: cover;
  background-position: center;
  box-shadow: 0 18px 40px rgba(0, 0, 0, 0.28);
}

.rsvp-dialog::backdrop {
  background-color: rgba(26, 26, 26, 0.55);
  backdrop-filter: blur(3px);
}

.rsvp-close {
  position: absolute;
  top: 0.4rem;
  right: 0.7rem;
  font-size: 1.7rem;
  line-height: 1;
  color: #5c5c5c;
}

.field {
  display: flex;
  flex-direction: column;
  gap: 0.35rem;
  text-align: left;
}

.field span {
  font-family: var(--font-typewriter);
  font-size: 0.72rem;
  font-weight: 700;
  letter-spacing: 0.16em;
  text-transform: uppercase;
}

.field input {
  appearance: none;
  width: 100%;
  height: 3.5rem;
  border: none;
  border-radius: 0;
  border-bottom: 1.5px solid #1a1a1a;
  background: transparent;
  padding: 0.35rem 0.1rem 0.15rem;
  font-family: var(--font-script);
  font-size: 2.75rem;
  line-height: 1;
  color: var(--color-ink);
  outline: none;
}

.field input:focus {
  border-bottom-color: #8a6a3d;
}

.age-check {
  display: flex;
  align-items: flex-start;
  gap: 0.75rem;
  text-align: left;
  font-family: var(--font-typewriter);
  font-size: 0.88rem;
  line-height: 1.35;
  cursor: pointer;
}

.age-check input {
  appearance: none;
  flex-shrink: 0;
  width: 1.15rem;
  height: 1.15rem;
  margin: 0.1rem 0 0;
  border: 1.5px solid #1a1a1a;
  background: rgba(255, 255, 255, 0.35);
  cursor: pointer;
}

.age-check input:checked {
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 12 12'%3E%3Cpath fill='none' stroke='%231a1a1a' stroke-width='2' d='M2 6.5 5 9.5 10 3'/%3E%3C/svg%3E");
  background-repeat: no-repeat;
  background-position: center;
  background-size: 0.85rem;
}

.age-check input:focus-visible {
  outline: 2px solid #8a6a3d;
  outline-offset: 2px;
}

.rsvp-btn {
  min-width: 12rem;
  min-height: 2.75rem;
  padding: 0.7rem 1.4rem;
  border: 1.5px solid #1a1a1a;
  background: rgba(255, 255, 255, 0.28);
  font-family: var(--font-typewriter);
  font-size: 0.78rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
}

.rsvp-btn:disabled {
  opacity: 0.45;
  cursor: not-allowed;
}

.rsvp-link {
  font-family: var(--font-typewriter);
  font-size: 0.8rem;
  text-decoration: underline;
  text-underline-offset: 0.2rem;
  color: #5c5c5c;
}

.calendar-card {
  width: 100%;
  padding: 1.15rem 1rem 1.25rem;
  background: rgba(255, 255, 255, 0.38);
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.08);
}
</style>
