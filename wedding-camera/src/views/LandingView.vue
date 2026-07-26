<script setup>
import { computed, onMounted, ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

const guestName = ref('')
const submitted = ref(false)

const STORAGE_KEYS = {
  guestName: 'paul_60_guest_name',
}

const cleanedGuestName = computed(() => {
  return guestName.value.trim().replace(/\s+/g, ' ')
})

const nameError = computed(() => {
  if (!submitted.value) return ''
  if (!cleanedGuestName.value) return 'Please enter your name to begin.'
  if (cleanedGuestName.value.length < 2) return 'Please enter at least 2 characters.'
  if (cleanedGuestName.value.length > 60) return 'Please keep your name under 60 characters.'
  return ''
})

function beginCamera() {
  submitted.value = true

  if (nameError.value) return

  localStorage.setItem(STORAGE_KEYS.guestName, cleanedGuestName.value)
  router.push('/camera')
}

onMounted(() => {
  guestName.value = localStorage.getItem(STORAGE_KEYS.guestName) || ''
})
</script>

<template>
  <main class="landing-page">
    <div class="landing-shell">
      <div class="leopard-pattern" aria-hidden="true"></div>

      <header class="event-header">
        <p class="eyebrow">Paul Peters</p>

        <h1>
          <span>Celebrating</span>
          <strong>60 Years</strong>
          <span class="title-tail">of Paul</span>
        </h1>

        <div class="birthday-mark" aria-hidden="true">
          <span>60</span>
        </div>

        <p class="event-details">1 August <span>•</span> Masonic Hall, Leigh</p>
      </header>

      <form class="film-frame" @submit.prevent="beginCamera">
        <div class="film-rail film-rail--left" aria-hidden="true">
          <span>ISO 400</span>
          <span>PP 60</span>
        </div>

        <div class="name-panel">
          <p class="frame-label">Your disposable camera</p>
          <h2>Who’s behind the lens?</h2>
          <p class="frame-copy">
            Enter your name so Paul knows who captured each memory.
          </p>

          <label for="guest-name">Your name</label>
          <input
            id="guest-name"
            v-model="guestName"
            type="text"
            name="guest-name"
            autocomplete="name"
            enterkeyhint="go"
            maxlength="60"
            placeholder="e.g. Jonny Whittle"
            :aria-invalid="Boolean(nameError)"
            :aria-describedby="nameError ? 'name-error' : undefined"
          />

          <p v-if="nameError" id="name-error" class="name-error" role="alert">
            {{ nameError }}
          </p>
        </div>

        <div class="film-rail film-rail--right" aria-hidden="true">
          <i v-for="index in 4" :key="index"></i>
        </div>
      </form>

      <button class="camera-ticket" type="button" @click="beginCamera">
        <span class="shot-stub">
          <strong>25</strong>
          <small>Shots</small>
        </span>

        <span class="start-label">Start the camera</span>

        <span class="ticket-mark" aria-hidden="true">60</span>
      </button>

      <p class="tagline">
        <span></span>
        Disposable memories last a lifetime
        <span></span>
      </p>

      <RouterLink class="admin-link" to="/admin">Admin</RouterLink>
    </div>
  </main>
</template>

<style scoped lang="scss">
.landing-page {
  --ink: #0d0d0c;
  --paper: #d9c7a5;
  --muted-paper: #bba274;
  --gold: #c6a15e;
  --deep-gold: #8d6934;

  min-height: 100svh;
  overflow: hidden;
  background:
    radial-gradient(circle at 50% -10%, rgba(198, 161, 94, 0.1), transparent 35%),
    #050505;
  color: var(--paper);
}

.landing-shell {
  position: relative;
  isolation: isolate;
  display: flex;
  min-height: 100svh;
  width: min(100%, 30rem);
  margin: 0 auto;
  padding: max(2rem, env(safe-area-inset-top)) 1.35rem max(1rem, env(safe-area-inset-bottom));
  flex-direction: column;
  background:
    linear-gradient(115deg, transparent 0 45%, rgba(255, 255, 255, 0.012) 50%, transparent 55%),
    #11110f;
  box-shadow: 0 0 70px rgba(0, 0, 0, 0.85);
}

.landing-shell::after {
  position: absolute;
  z-index: -1;
  inset: 0;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 180 180' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.9' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='.17'/%3E%3C/svg%3E");
  content: '';
  opacity: 0.2;
  pointer-events: none;
}

.leopard-pattern {
  position: absolute;
  z-index: -1;
  top: 0;
  left: 0;
  width: 100%;
  height: 11rem;
  opacity: 0.2;
  background:
    radial-gradient(ellipse at 12% 25%, transparent 0 6px, #c6a15e 7px 9px, transparent 10px),
    radial-gradient(ellipse at 42% 18%, transparent 0 8px, #c6a15e 9px 11px, transparent 12px),
    radial-gradient(ellipse at 72% 32%, transparent 0 5px, #c6a15e 6px 8px, transparent 9px),
    radial-gradient(ellipse at 91% 14%, transparent 0 7px, #c6a15e 8px 10px, transparent 11px);
  background-size: 8.5rem 5rem;
  mask-image: linear-gradient(to bottom, black, transparent);
}

.event-header {
  text-align: center;
}

.eyebrow {
  margin: 0 0 1.1rem;
  color: var(--gold);
  font-size: 0.72rem;
  font-weight: 700;
  letter-spacing: 0.3em;
  text-transform: uppercase;
}

h1 {
  margin: 0;
  font-family: Georgia, 'Times New Roman', serif;
  font-weight: 400;
  letter-spacing: 0.035em;
  line-height: 0.92;
  text-transform: uppercase;
}

h1 span,
h1 strong {
  display: block;
}

h1 > span:first-child {
  color: var(--gold);
  font-size: clamp(2rem, 8.5vw, 2.7rem);
}

h1 strong {
  margin: 0.2rem 0;
  color: #e5d7be;
  font-size: clamp(3.2rem, 14vw, 4.6rem);
  font-weight: 400;
}

h1 .title-tail {
  display: flex;
  align-items: center;
  gap: 0.8rem;
  color: var(--gold);
  font-size: clamp(1.65rem, 7vw, 2.2rem);
}

h1 .title-tail::before,
h1 .title-tail::after {
  height: 1px;
  flex: 1;
  background: linear-gradient(to right, transparent, var(--deep-gold));
  content: '';
}

h1 .title-tail::after {
  background: linear-gradient(to left, transparent, var(--deep-gold));
}

.birthday-mark {
  display: grid;
  width: 2.9rem;
  height: 3.2rem;
  margin: 1rem auto 0.8rem;
  place-items: center;
  border: 1px solid var(--gold);
  border-radius: 48% 48% 54% 54% / 38% 38% 62% 62%;
  color: var(--gold);
  font-family: Georgia, 'Times New Roman', serif;
  font-size: 1rem;
  transform: perspective(4rem) rotateX(-7deg);
}

.event-details {
  margin: 0;
  color: #ddc998;
  font-size: clamp(0.68rem, 2.9vw, 0.82rem);
  font-weight: 700;
  letter-spacing: 0.13em;
  text-transform: uppercase;
}

.event-details span {
  margin: 0 0.35rem;
  color: var(--gold);
}

.film-frame {
  position: relative;
  display: grid;
  min-height: 17rem;
  margin-top: 1.5rem;
  grid-template-columns: 1.8rem minmax(0, 1fr) 1.8rem;
  overflow: hidden;
  border: 2px solid var(--gold);
  background:
    linear-gradient(rgba(10, 10, 9, 0.94), rgba(10, 10, 9, 0.94)),
    repeating-linear-gradient(135deg, #191814 0 4px, #11110f 4px 8px);
  box-shadow:
    inset 0 0 0 4px var(--ink),
    inset 0 0 0 5px rgba(198, 161, 94, 0.6),
    0 0.8rem 2rem rgba(0, 0, 0, 0.35);
}

.film-frame::before,
.film-frame::after {
  position: absolute;
  width: 2.2rem;
  height: 0.35rem;
  background: var(--ink);
  content: '';
}

.film-frame::before {
  top: -0.1rem;
  left: 24%;
}

.film-frame::after {
  right: 18%;
  bottom: -0.1rem;
}

.film-rail {
  display: flex;
  align-items: center;
  justify-content: space-around;
  color: var(--gold);
  font-size: 0.58rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  writing-mode: vertical-rl;
}

.film-rail--left {
  border-right: 1px solid rgba(198, 161, 94, 0.55);
  transform: rotate(180deg);
}

.film-rail--right {
  border-left: 1px solid rgba(198, 161, 94, 0.55);
}

.film-rail i {
  display: block;
  width: 0.45rem;
  height: 0.7rem;
  border: 1px solid var(--gold);
  border-radius: 1px;
}

.name-panel {
  display: flex;
  padding: 1.6rem 1rem;
  flex-direction: column;
  justify-content: center;
  text-align: center;
}

.frame-label {
  margin: 0 0 0.65rem;
  color: var(--gold);
  font-size: 0.62rem;
  font-weight: 700;
  letter-spacing: 0.2em;
  text-transform: uppercase;
}

.name-panel h2 {
  margin: 0;
  color: #ecdfc8;
  font-family: Georgia, 'Times New Roman', serif;
  font-size: 1.55rem;
  font-weight: 400;
}

.frame-copy {
  max-width: 17rem;
  margin: 0.65rem auto 1.2rem;
  color: #b9ab94;
  font-size: 0.78rem;
  line-height: 1.5;
}

.name-panel label {
  position: absolute;
  width: 1px;
  height: 1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
}

.name-panel input {
  width: 100%;
  border: 1px solid rgba(198, 161, 94, 0.7);
  border-radius: 0;
  outline: none;
  background: rgba(217, 199, 165, 0.05);
  padding: 0.85rem 1rem;
  color: #f2e7d5;
  font-size: 1rem;
  text-align: center;
  transition:
    border-color 160ms ease,
    box-shadow 160ms ease,
    background 160ms ease;
}

.name-panel input::placeholder {
  color: #776d5e;
}

.name-panel input:focus {
  border-color: #e2c98d;
  background: rgba(217, 199, 165, 0.09);
  box-shadow: 0 0 0 3px rgba(198, 161, 94, 0.13);
}

.name-panel input[aria-invalid='true'] {
  border-color: #c66e62;
}

.name-error {
  margin: 0.55rem 0 -0.4rem;
  color: #e19b90;
  font-size: 0.72rem;
}

.camera-ticket {
  display: grid;
  width: 100%;
  min-height: 4.8rem;
  margin-top: 1rem;
  padding: 0;
  grid-template-columns: 4.6rem minmax(0, 1fr) 3.4rem;
  align-items: stretch;
  overflow: hidden;
  border: 1px solid #e1c487;
  border-radius: 0;
  background:
    linear-gradient(100deg, rgba(255, 255, 255, 0.08), transparent 24% 75%, rgba(69, 42, 8, 0.16)),
    var(--gold);
  color: #17130c;
  box-shadow: 0 0.75rem 1.8rem rgba(0, 0, 0, 0.35);
  cursor: pointer;
  text-transform: uppercase;
}

.camera-ticket:active {
  transform: translateY(1px);
}

.shot-stub,
.ticket-mark {
  display: flex;
  align-items: center;
  justify-content: center;
}

.shot-stub {
  position: relative;
  flex-direction: column;
  border-right: 1px dashed rgba(23, 19, 12, 0.55);
}

.shot-stub::before,
.shot-stub::after {
  position: absolute;
  right: -0.3rem;
  width: 0.6rem;
  height: 0.6rem;
  border-radius: 50%;
  background: #11110f;
  content: '';
}

.shot-stub::before {
  top: -0.3rem;
}

.shot-stub::after {
  bottom: -0.3rem;
}

.shot-stub strong {
  font-family: Georgia, 'Times New Roman', serif;
  font-size: 2rem;
  font-weight: 400;
  line-height: 0.9;
}

.shot-stub small {
  margin-top: 0.3rem;
  font-size: 0.58rem;
  font-weight: 800;
  letter-spacing: 0.12em;
}

.start-label {
  display: grid;
  place-items: center;
  border: 1px solid rgba(23, 19, 12, 0.55);
  margin: 0.75rem 0.6rem;
  font-size: clamp(0.82rem, 3.6vw, 1rem);
  font-weight: 900;
  letter-spacing: 0.1em;
}

.ticket-mark {
  border-left: 1px solid rgba(23, 19, 12, 0.35);
  font-family: Georgia, 'Times New Roman', serif;
  font-size: 1.2rem;
}

.tagline {
  display: flex;
  margin: 1.15rem 0 0;
  align-items: center;
  gap: 0.7rem;
  color: var(--gold);
  font-size: 0.62rem;
  font-weight: 600;
  letter-spacing: 0.15em;
  line-height: 1.45;
  text-align: center;
  text-transform: uppercase;
}

.tagline span {
  height: 1px;
  flex: 1;
  background: var(--deep-gold);
}

.admin-link {
  align-self: center;
  margin-top: auto;
  padding: 1rem 1rem 0;
  color: #786b55;
  font-size: 0.68rem;
  letter-spacing: 0.12em;
  text-decoration: none;
  text-transform: uppercase;
}

@media (max-height: 760px) {
  .landing-shell {
    padding-top: 1rem;
  }

  .eyebrow {
    margin-bottom: 0.65rem;
  }

  .birthday-mark {
    display: none;
  }

  .film-frame {
    min-height: 14rem;
    margin-top: 1rem;
  }

  .name-panel {
    padding-block: 1rem;
  }

  .tagline {
    margin-top: 0.8rem;
  }
}
</style>
