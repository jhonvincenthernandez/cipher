
<template>
  <IonPage>
    <!-- ==================== HEADER ==================== -->
    <IonHeader v-if="currentView !== 'landing'">
      <IonToolbar class="app-toolbar">
        <IonTitle class="app-title">
          Caesar Cipher
        </IonTitle>
      </IonToolbar>
    </IonHeader>

    <!-- ==================== LANDING VIEW ==================== -->
    <IonContent
      v-if="currentView === 'landing'"
      :fullscreen="true"
      class="landing-content"
    >
      <main class="landing-container">
        <section class="landing-hero">
          <div
            class="landing-icon"
            aria-hidden="true"
          >
            ⌘
          </div>

          <p class="landing-eyebrow">
            CRYPTOGRAPHY PROJECT
          </p>

          <h1>
            Encrypt or Decrypt
          </h1>

          <p class="landing-description">
            Transform your message using the
            Caesar Shift Cipher.
          </p>

          <p class="landing-developer">
            Developed by
            <strong>
              Hernandez, Jhon Vincent O.
            </strong>
          </p>

          <IonButton
            expand="block"
            class="get-started-button"
            @click="goToView('home')"
          >
            Get Started
            <span
              class="button-arrow"
              aria-hidden="true"
            >
              →
            </span>
          </IonButton>

          <p class="landing-note">
            Simple shifts. Secure learning.
          </p>
        </section>

        <section class="landing-features">
          <div class="landing-feature">
            <span aria-hidden="true">↗</span>
            <strong>Encrypt</strong>
            <p>Convert plaintext into ciphertext.</p>
          </div>

          <div class="landing-feature">
            <span aria-hidden="true">↙</span>
            <strong>Decrypt</strong>
            <p>Convert ciphertext back to plaintext.</p>
          </div>

          <div class="landing-feature">
            <span aria-hidden="true">⌘</span>
            <strong>Learn</strong>
            <p>Understand the Caesar Cipher process.</p>
          </div>
        </section>
      </main>
    </IonContent>

    <!-- ==================== HOME VIEW ==================== -->
    <IonContent
      v-else-if="currentView === 'home'"
      :fullscreen="true"
    >
      <main class="page-container">
        <!-- Hero Section -->
        <section class="hero-section">
          <div
            class="hero-icon"
            aria-hidden="true"
          >
            <span>⌘</span>
          </div>

          <p class="eyebrow">
            CRYPTOGRAPHY PROJECT
          </p>

          <h1>
            Encrypt or Decrypt
          </h1>

          <p class="hero-description">
            Transform your message using the
            Caesar Shift Cipher.
          </p>
        </section>

        <!-- Main Operation Card -->
        <IonCard class="app-card operation-card">
          <IonCardContent>
            <!-- Mode Selector -->
            <div class="section-heading">
              <span class="section-number">01</span>
              <span class="section-title">
                Select Operation
              </span>
            </div>

            <IonSegment
              v-model="mode"
              class="mode-segment"
              aria-label="Select cipher operation"
            >
              <IonSegmentButton value="encrypt">
                <IonLabel>
                  <span class="segment-icon">↗</span>
                  Encrypt
                </IonLabel>
              </IonSegmentButton>

              <IonSegmentButton value="decrypt">
                <IonLabel>
                  <span class="segment-icon">↙</span>
                  Decrypt
                </IonLabel>
              </IonSegmentButton>
            </IonSegment>

            <!-- Message Input -->
            <div class="section-heading input-heading">
              <span class="section-number">02</span>
              <span class="section-title">
                Enter Message
              </span>
            </div>

            <IonItem
              lines="none"
              class="input-item"
            >
              <IonLabel
                position="stacked"
                class="field-label"
              >
                {{ inputLabel }}
              </IonLabel>

              <IonTextarea
                v-model="inputText"
                :placeholder="inputPlaceholder"
                :auto-grow="true"
                :rows="4"
                :maxlength="5000"
                class="message-input"
                :aria-label="inputLabel"
              />
            </IonItem>

            <div class="character-counter">
              {{ inputText.length }} / 5000 characters
            </div>

            <!-- Shift Input -->
            <div class="section-heading input-heading">
              <span class="section-number">03</span>
              <span class="section-title">
                Set Shift Value
              </span>
            </div>

            <IonItem
              lines="none"
              class="input-item"
            >
              <IonLabel
                position="stacked"
                class="field-label"
              >
                Shift Number
              </IonLabel>

              <IonInput
                v-model="shiftText"
                type="number"
                min="1"
                max="25"
                inputmode="numeric"
                placeholder="Enter a value from 1 to 25"
                class="shift-input"
                aria-label="Shift value from 1 to 25"
              />
            </IonItem>

            <p
              v-if="shiftError !== ''"
              class="error-message"
              role="alert"
            >
              <span aria-hidden="true">!</span>
              {{ shiftError }}
            </p>

            <p
              v-else
              class="field-hint"
            >
              Use a number from 1 to 25.
            </p>

            <!-- Main Action Button -->
            <IonButton
              expand="block"
              class="primary-button"
              :disabled="!canProcess"
              @click="processCipher"
            >
              <span
                class="button-symbol"
                aria-hidden="true"
              >
                {{ isEncryptMode ? '↗' : '↙' }}
              </span>

              {{ isEncryptMode ? 'Encrypt Message' : 'Decrypt Message' }}
            </IonButton>
          </IonCardContent>
        </IonCard>

        <!-- Result Card -->
        <IonCard
          v-if="resultText !== ''"
          class="app-card result-card"
        >
          <IonCardContent>
            <div class="result-heading">
              <div>
                <p class="result-eyebrow">
                  OPERATION COMPLETE
                </p>

                <h2>
                  {{ outputLabel }}
                </h2>
              </div>

              <button
                type="button"
                class="copy-button"
                aria-label="Copy result"
                @click="copyResult"
              >
                <span aria-hidden="true">⧉</span>
              </button>
            </div>

            <div
              class="result-box"
              aria-live="polite"
            >
              {{ resultText }}
            </div>

            <p
              v-if="copyStatus !== ''"
              class="copy-status"
              role="status"
            >
              {{ copyStatus }}
            </p>

            <div class="success-message">
              <span
                class="success-icon"
                aria-hidden="true"
              >
                ✓
              </span>

              <span>
                Message
                {{ isEncryptMode ? 'encrypted' : 'decrypted' }}
                successfully.
              </span>
            </div>
          </IonCardContent>
        </IonCard>

        <!-- Cipher Process Card -->
        <IonCard class="app-card process-card">
          <IonCardHeader>
            <div class="section-heading process-heading">
              <span class="section-number">04</span>

              <IonCardTitle>
                Cipher Process
              </IonCardTitle>
            </div>
          </IonCardHeader>

          <IonCardContent>
            <div class="process-flow">
              <div class="process-step input-step">
                <span class="step-label">
                  {{ inputLabel }}
                </span>

                <strong>
                  {{ processInput }}
                </strong>
              </div>

              <span
                class="process-arrow"
                aria-hidden="true"
              >
                →
              </span>

              <div class="process-step shift-step">
                <span class="step-label">
                  Shift
                </span>

                <strong>
                  {{ processShift }}
                </strong>
              </div>

              <span
                class="process-arrow"
                aria-hidden="true"
              >
                →
              </span>

              <div class="process-step output-step">
                <span class="step-label">
                  {{ outputLabel }}
                </span>

                <strong>
                  {{ processOutput }}
                </strong>
              </div>
            </div>

            <div class="process-description">
              <span class="description-icon">ⓘ</span>
              {{ processDescription }}
            </div>

            <!-- Letter Example -->
            <div class="letter-example">
              <p class="example-label">
                LETTER EXAMPLE
              </p>

              <div class="letter-flow">
                <strong>{{ exampleInput }}</strong>

                <span>→</span>

                <strong>{{ exampleOutput }}</strong>
              </div>

              <p>
                {{ exampleDescription }}
              </p>
            </div>
          </IonCardContent>
        </IonCard>

        <!-- Clear Button -->
        <IonButton
          expand="block"
          fill="outline"
          class="clear-button"
          @click="clearForm"
        >
          <span
            class="clear-symbol"
            aria-hidden="true"
          >
            ↻
          </span>

          Clear All
        </IonButton>

        <!-- Developer Credit -->
        <p class="developer-credit">
          Developed by
          <strong>
            Hernandez, Jhon Vincent O.
          </strong>
        </p>
      </main>
    </IonContent>

    <!-- ==================== ABOUT VIEW ==================== -->
    <IonContent
      v-else
      :fullscreen="true"
    >
      <main class="page-container about-container">
        <!-- About Hero -->
        <section class="hero-section about-hero">
          <div
            class="hero-icon"
            aria-hidden="true"
          >
            ⓘ
          </div>

          <p class="eyebrow">
            APPLICATION INFORMATION
          </p>

          <h1>
            About This App
          </h1>

          <p class="hero-description">
            Learn how the Caesar Cipher works.
          </p>
        </section>

        <!-- About Card -->
        <IonCard class="app-card">
          <IonCardContent>
            <h2 class="about-title">
              Caesar Cipher
            </h2>

            <p class="about-description">
              The Caesar Cipher is a substitution
              encryption technique that shifts each
              letter by a fixed number of positions
              in the alphabet.
            </p>

            <!-- Encryption Formula -->
            <div class="formula-section">
              <p class="formula-label">
                ENCRYPTION FORMULA
              </p>

              <div class="formula-box">
                C = (P + S) mod 26
              </div>
            </div>

            <!-- Decryption Formula -->
            <div class="formula-section">
              <p class="formula-label">
                DECRYPTION FORMULA
              </p>

              <div class="formula-box">
                P = (C - S) mod 26
              </div>
            </div>

            <!-- Formula Definitions -->
            <div class="definitions-box">
              <div class="definition-row">
                <strong>C</strong>
                <span>Ciphertext letter</span>
              </div>

              <div class="definition-row">
                <strong>P</strong>
                <span>Plaintext letter</span>
              </div>

              <div class="definition-row">
                <strong>S</strong>
                <span>Shift value</span>
              </div>

              <div class="definition-row">
                <strong>mod 26</strong>
                <span>Wraps the alphabet</span>
              </div>
            </div>

            <div class="content-divider"></div>

            <!-- Example -->
            <h3 class="subsection-title">
              Example
            </h3>

            <div class="example-box">
              <div class="example-row">
                <span>Plaintext</span>
                <strong>HELLO</strong>
              </div>

              <div class="example-row">
                <span>Shift</span>
                <strong>+3</strong>
              </div>

              <div class="example-row">
                <span>Ciphertext</span>
                <strong>KHOOR</strong>
              </div>
            </div>

            <div class="content-divider"></div>

            <!-- Supported Characters -->
            <h3 class="subsection-title">
              Supported Characters
            </h3>

            <ul class="feature-list">
              <li>Uppercase letters: A–Z</li>
              <li>Lowercase letters: a–z</li>
              <li>Spaces and punctuation remain unchanged</li>
              <li>Numbers and special characters remain unchanged</li>
            </ul>

            <div class="content-divider"></div>

            <!-- Developer Information -->
            <p class="developer-label">
              DEVELOPED BY
            </p>

            <h3 class="developer-name">
              Hernandez, Jhon Vincent O.
            </h3>

            <p class="developer-role">
              Bachelor of Science in Information Technology
            </p>
          </IonCardContent>
        </IonCard>

        <!-- Quote -->
        <div class="quote-box">
          “Simple Shifts, Stronger Thinking.”
        </div>
      </main>
    </IonContent>

    <!-- ==================== BOTTOM NAVIGATION ==================== -->
    <IonFooter v-if="currentView !== 'landing'">
      <IonToolbar class="bottom-toolbar">
        <nav
          class="bottom-navigation"
          aria-label="Main navigation"
        >
          <!-- Home -->
          <button
            type="button"
            class="nav-button"
            :class="{ active: currentView === 'home' }"
            :aria-current="
              currentView === 'home'
                ? 'page'
                : undefined
            "
            @click="goToView('home')"
          >
            <span
              class="nav-icon"
              aria-hidden="true"
            >
              ⌂
            </span>

            <span>Home</span>
          </button>

          <!-- About -->
          <button
            type="button"
            class="nav-button"
            :class="{ active: currentView === 'about' }"
            :aria-current="
              currentView === 'about'
                ? 'page'
                : undefined
            "
            @click="goToView('about')"
          >
            <span
              class="nav-icon"
              aria-hidden="true"
            >
              ⓘ
            </span>

            <span>About</span>
          </button>
        </nav>
      </IonToolbar>
    </IonFooter>
  </IonPage>
</template>

<script setup lang="ts">
import {
  computed,
  ref,
  watch
} from 'vue';

import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
  IonFooter,
  IonCard,
  IonCardHeader,
  IonCardTitle,
  IonCardContent,
  IonSegment,
  IonSegmentButton,
  IonLabel,
  IonTextarea,
  IonInput,
  IonItem,
  IonButton
} from '@ionic/vue';

/* =========================================================
   TYPES
========================================================= */

type CipherMode = 'encrypt' | 'decrypt';

type AppView = 'landing' | 'home' | 'about';

/* =========================================================
   STATE
========================================================= */

const currentView = ref<AppView>('landing');

const mode = ref<CipherMode>('encrypt');

const inputText = ref('');

const shiftText = ref('3');

const resultText = ref('');

const copyStatus = ref('');

/* =========================================================
   MODE COMPUTED VALUES
========================================================= */

const isEncryptMode = computed(() => {
  return mode.value === 'encrypt';
});

const inputLabel = computed(() => {
  return isEncryptMode.value
    ? 'Plaintext'
    : 'Ciphertext';
});

const outputLabel = computed(() => {
  return isEncryptMode.value
    ? 'Ciphertext'
    : 'Plaintext';
});

const inputPlaceholder = computed(() => {
  return isEncryptMode.value
    ? 'Type your plaintext message here...'
    : 'Type your ciphertext message here...';
});

/* =========================================================
   SHIFT VALIDATION
========================================================= */

const parsedShift = computed<number | null>(() => {
  const trimmedShift = shiftText.value.trim();

  if (trimmedShift === '') {
    return null;
  }

  const numericShift = Number(trimmedShift);

  if (!Number.isInteger(numericShift)) {
    return null;
  }

  return numericShift;
});

const isShiftValid = computed(() => {
  return (
    parsedShift.value !== null &&
    parsedShift.value >= 1 &&
    parsedShift.value <= 25
  );
});

const validShift = computed(() => {
  if (!isShiftValid.value) {
    return 0;
  }

  return parsedShift.value as number;
});

const shiftError = computed(() => {
  if (shiftText.value.trim() === '') {
    return 'Shift value is required.';
  }

  if (parsedShift.value === null) {
    return 'Shift must be a whole number.';
  }

  if (
    parsedShift.value < 1 ||
    parsedShift.value > 25
  ) {
    return 'Shift must be between 1 and 25.';
  }

  return '';
});

/* =========================================================
   INPUT VALIDATION
========================================================= */

const canProcess = computed(() => {
  return (
    inputText.value.trim() !== '' &&
    isShiftValid.value
  );
});

/* =========================================================
   CAESAR CIPHER ALGORITHM
========================================================= */

const caesarCipher = (
  text: string,
  shift: number,
  decrypt: boolean
): string => {
  const direction = decrypt
    ? -shift
    : shift;

  return text
    .split('')
    .map((character) => {
      const characterCode = character.charCodeAt(0);

      // Uppercase letters: A-Z
      if (
        characterCode >= 65 &&
        characterCode <= 90
      ) {
        const shiftedCode =
          (
            (characterCode - 65 + direction) % 26 + 26
          ) % 26;

        return String.fromCharCode(
          shiftedCode + 65
        );
      }

      // Lowercase letters: a-z
      if (
        characterCode >= 97 &&
        characterCode <= 122
      ) {
        const shiftedCode =
          (
            (characterCode - 97 + direction) % 26 + 26
          ) % 26;

        return String.fromCharCode(
          shiftedCode + 97
        );
      }

      // Preserve numbers, spaces,
      // punctuation, and special characters.
      return character;
    })
    .join('');
};

/* =========================================================
   PROCESS PREVIEW
========================================================= */

const processInput = computed(() => {
  if (inputText.value.trim() === '') {
    return 'Input';
  }

  return inputText.value;
});

const processShift = computed(() => {
  if (!isShiftValid.value) {
    return 'Invalid';
  }

  return isEncryptMode.value
    ? `+${validShift.value}`
    : `-${validShift.value}`;
});

const processOutput = computed(() => {
  if (inputText.value.trim() === '') {
    return 'Output';
  }

  if (!isShiftValid.value) {
    return 'Invalid';
  }

  return caesarCipher(
    inputText.value,
    validShift.value,
    !isEncryptMode.value
  );
});

const processDescription = computed(() => {
  if (inputText.value.trim() === '') {
    return 'Enter a message to see the cipher process.';
  }

  if (!isShiftValid.value) {
    return 'Enter a valid shift from 1 to 25.';
  }

  if (isEncryptMode.value) {
    return `Each letter moves ${validShift.value} position(s) forward in the alphabet.`;
  }

  return `Each letter moves ${validShift.value} position(s) backward in the alphabet.`;
});

/* =========================================================
   LETTER EXAMPLE
========================================================= */

const exampleInput = computed(() => {
  return isEncryptMode.value
    ? 'A'
    : 'D';
});

const exampleOutput = computed(() => {
  if (!isShiftValid.value) {
    return '?';
  }

  return isEncryptMode.value
    ? caesarCipher('A', validShift.value, false)
    : caesarCipher('D', validShift.value, true);
});

const exampleDescription = computed(() => {
  if (!isShiftValid.value) {
    return 'Enter a valid shift to view an example.';
  }

  if (isEncryptMode.value) {
    return `A moves ${validShift.value} position(s) forward.`;
  }

  return `D moves ${validShift.value} position(s) backward.`;
});

/* =========================================================
   ACTIONS
========================================================= */

const processCipher = () => {
  if (!canProcess.value) {
    resultText.value = '';
    return;
  }

  resultText.value = caesarCipher(
    inputText.value,
    validShift.value,
    !isEncryptMode.value
  );

  copyStatus.value = '';
};

const clearForm = () => {
  inputText.value = '';
  shiftText.value = '3';
  resultText.value = '';
  copyStatus.value = '';
  mode.value = 'encrypt';
};

const goToView = (view: AppView) => {
  currentView.value = view;
};

const copyResult = async () => {
  if (resultText.value === '') {
    return;
  }

  if (
    !navigator.clipboard ||
    !navigator.clipboard.writeText
  ) {
    copyStatus.value =
      'Copy is not supported. Please copy manually.';

    return;
  }

  try {
    await navigator.clipboard.writeText(
      resultText.value
    );

    copyStatus.value =
      '✓ Result copied successfully.';
  } catch (error) {
    copyStatus.value =
      'Unable to copy. Please copy manually.';

    console.error(
      'Copy operation failed:',
      error
    );
  }
};

/* =========================================================
   WATCHERS
========================================================= */

watch(
  [mode, inputText, shiftText],
  () => {
    resultText.value = '';
    copyStatus.value = '';
  }
);
</script>

<style scoped>
/* =========================================================
   BASE LAYOUT
========================================================= */

.page-container,
.landing-container {
  width: 100%;
  max-width: 680px;
  margin: 0 auto;
  padding: 18px;
  padding-bottom: 30px;
}

.app-toolbar,
.bottom-toolbar {
  --background: var(--ion-background-color);
  --border-width: 0;
}

.app-title {
  font-size: 18px;
  font-weight: 800;
  letter-spacing: -0.3px;
}

.app-card {
  margin: 16px 0;
  border: 1px solid var(--ion-border-color);
  border-radius: 18px;
  background: var(--ion-card-background);
  box-shadow: 0 5px 20px rgba(15, 23, 42, 0.06);
}

.app-card ion-card-content {
  padding: 20px;
}

/* =========================================================
   LANDING PAGE
========================================================= */

.landing-content {
  --background:
    radial-gradient(
      circle at top right,
      rgba(124, 58, 237, 0.12),
      transparent 38%
    ),
    var(--ion-background-color);
}

.landing-container {
  display: flex;
  min-height: 100%;
  flex-direction: column;
  justify-content: center;
  padding-top: 35px;
  padding-bottom: 35px;
}

.landing-hero {
  padding: 15px 8px;
  text-align: center;
}

.landing-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 92px;
  height: 92px;
  margin: 0 auto 25px;
  border: 1px solid rgba(124, 92, 232, 0.25);
  border-radius: 28px;
  background: linear-gradient(
    145deg,
    #312e81,
    #7c3aed
  );
  box-shadow: 0 12px 35px rgba(99, 63, 190, 0.28);
  color: #ffffff;
  font-size: 50px;
  font-weight: 800;
}

.landing-eyebrow,
.eyebrow {
  margin: 0 0 9px;
  color: var(--ion-color-primary);
  font-size: 10px;
  font-weight: 850;
  letter-spacing: 1.7px;
}

.landing-hero h1 {
  max-width: 450px;
  margin: 0 auto;
  color: var(--ion-text-color);
  font-size: clamp(30px, 7vw, 43px);
  font-weight: 850;
  letter-spacing: -1.2px;
  line-height: 1.1;
}

.landing-description {
  max-width: 340px;
  margin: 17px auto 0;
  color: var(--ion-color-medium);
  font-size: 14px;
  line-height: 1.8;
}

.landing-developer {
  margin: 20px 0 0;
  color: var(--ion-color-medium);
  font-size: 11px;
  line-height: 1.8;
}

.landing-developer strong {
  display: block;
  color: var(--ion-text-color);
  font-size: 12px;
}

.get-started-button {
  max-width: 360px;
  min-height: 52px;
  margin: 30px auto 0;
  --border-radius: 13px;
  --background: linear-gradient(
    100deg,
    #4f46e5,
    #7c3aed
  );
  --box-shadow: 0 8px 22px rgba(99, 63, 190, 0.25);
  font-size: 13px;
  font-weight: 800;
}

.button-arrow {
  margin-left: 10px;
  font-size: 19px;
}

.landing-note {
  margin: 15px 0 0;
  color: var(--ion-color-medium);
  font-size: 10px;
  font-style: italic;
}

.landing-features {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 9px;
  margin-top: 35px;
}

.landing-feature {
  padding: 15px 8px;
  border: 1px solid var(--ion-border-color);
  border-radius: 13px;
  background: var(--ion-card-background);
  text-align: center;
}

.landing-feature > span {
  display: block;
  margin-bottom: 7px;
  color: var(--ion-color-primary);
  font-size: 21px;
  font-weight: 800;
}

.landing-feature strong {
  display: block;
  color: var(--ion-text-color);
  font-size: 11px;
  font-weight: 800;
}

.landing-feature p {
  margin: 7px 0 0;
  color: var(--ion-color-medium);
  font-size: 9px;
  line-height: 1.5;
}

/* =========================================================
   HERO SECTION
========================================================= */

.hero-section {
  padding: 28px 12px 18px;
  text-align: center;
}

.hero-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 72px;
  height: 72px;
  margin: 0 auto 18px;
  border: 1px solid rgba(124, 92, 232, 0.2);
  border-radius: 22px;
  background: linear-gradient(
    145deg,
    #312e81,
    #7c3aed
  );
  box-shadow: 0 8px 24px rgba(99, 63, 190, 0.22);
  color: #ffffff;
  font-size: 38px;
  font-weight: 700;
}

.hero-section h1 {
  margin: 0;
  color: var(--ion-text-color);
  font-size: 27px;
  font-weight: 850;
  letter-spacing: -0.8px;
}

.hero-description {
  max-width: 320px;
  margin: 10px auto 0;
  color: var(--ion-color-medium);
  font-size: 13px;
  line-height: 1.6;
}

/* =========================================================
   SECTION HEADINGS
========================================================= */

.section-heading {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 13px;
}

.section-number {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: 27px;
  height: 23px;
  border-radius: 7px;
  background: rgba(124, 92, 232, 0.12);
  color: var(--ion-color-primary);
  font-size: 10px;
  font-weight: 850;
}

.section-title {
  color: var(--ion-text-color);
  font-size: 13px;
  font-weight: 800;
}

.input-heading {
  margin-top: 25px;
}

.process-heading {
  margin-bottom: 0;
}

.process-heading ion-card-title {
  color: var(--ion-text-color);
  font-size: 16px;
  font-weight: 800;
}

/* =========================================================
   MODE SELECTOR
========================================================= */

.mode-segment {
  min-height: 48px;
  border: 1px solid var(--ion-border-color);
  border-radius: 12px;
  background: var(--ion-background-color);
}

.mode-segment ion-segment-button {
  min-height: 43px;
  --indicator-color: var(--ion-color-primary);
  --color: var(--ion-color-medium);
  --color-checked: #ffffff;
  font-size: 12px;
  font-weight: 750;
}

.segment-icon {
  margin-right: 5px;
  font-size: 15px;
  font-weight: 800;
}

/* =========================================================
   INPUT FIELDS
========================================================= */

.input-item {
  --background: transparent;
  --padding-start: 0;
  --inner-padding-end: 0;
  --inner-border-width: 0;
  margin: 0;
}

.field-label {
  margin-bottom: 9px;
  color: var(--ion-text-color) !important;
  font-size: 12px !important;
  font-weight: 750 !important;
}

.message-input,
.shift-input {
  width: 100%;
  border: 1px solid var(--ion-border-color);
  border-radius: 11px;
  background: var(--ion-background-color);
  color: var(--ion-text-color);
  --padding-start: 13px;
  --padding-end: 13px;
}

.message-input {
  --padding-top: 12px;
  --padding-bottom: 12px;
  line-height: 1.5;
}

.shift-input {
  min-height: 47px;
  --padding-top: 10px;
  --padding-bottom: 10px;
}

.character-counter {
  margin-top: 7px;
  color: var(--ion-color-medium);
  font-size: 10px;
  text-align: right;
}

.field-hint {
  margin: 7px 0 0;
  color: var(--ion-color-medium);
  font-size: 11px;
}

.error-message {
  display: flex;
  align-items: center;
  gap: 7px;
  margin: 8px 0 0;
  color: var(--ion-color-danger);
  font-size: 11px;
  line-height: 1.5;
}

.error-message span {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 16px;
  height: 16px;
  border-radius: 50%;
  background: rgba(220, 38, 38, 0.12);
  font-weight: 800;
}

/* =========================================================
   BUTTONS
========================================================= */

.primary-button {
  min-height: 48px;
  margin: 25px 0 0;
  --border-radius: 11px;
  --background: linear-gradient(
    100deg,
    #4f46e5,
    #7c3aed
  );
  --box-shadow: 0 6px 15px rgba(99, 63, 190, 0.2);
  font-size: 12px;
  font-weight: 800;
}

.primary-button[disabled] {
  opacity: 0.45;
}

.button-symbol {
  margin-right: 8px;
  font-size: 17px;
  font-weight: 800;
}

.clear-button {
  min-height: 44px;
  margin: 16px 0;
  --border-radius: 11px;
  --border-color: var(--ion-border-color);
  font-size: 12px;
  font-weight: 750;
}

.clear-symbol {
  margin-right: 7px;
  font-size: 17px;
}

/* =========================================================
   RESULT CARD
========================================================= */

.result-card {
  border: 1px solid rgba(22, 163, 74, 0.28);
}

.result-heading {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  margin-bottom: 13px;
}

.result-eyebrow {
  margin: 0 0 4px;
  color: var(--ion-color-success);
  font-size: 9px;
  font-weight: 850;
  letter-spacing: 1.2px;
}

.result-heading h2 {
  margin: 0;
  color: var(--ion-text-color);
  font-size: 17px;
  font-weight: 850;
}

.copy-button {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 38px;
  height: 38px;
  flex-shrink: 0;
  border: 1px solid var(--ion-border-color);
  border-radius: 10px;
  background: var(--ion-background-color);
  color: var(--ion-color-primary);
  font-size: 20px;
}

.copy-button:active {
  transform: scale(0.96);
}

.result-box {
  min-height: 50px;
  padding: 14px;
  border: 1px solid var(--ion-border-color);
  border-radius: 11px;
  background: var(--ion-background-color);
  color: var(--ion-text-color);
  font-size: 14px;
  font-weight: 650;
  line-height: 1.6;
  overflow-wrap: anywhere;
  white-space: pre-wrap;
}

.copy-status {
  margin: 9px 0 0;
  color: var(--ion-color-success);
  font-size: 11px;
  font-weight: 650;
}

.success-message {
  display: flex;
  align-items: center;
  gap: 9px;
  margin-top: 13px;
  padding: 11px;
  border-radius: 10px;
  background: rgba(22, 163, 74, 0.09);
  color: var(--ion-color-success);
  font-size: 11px;
  font-weight: 700;
}

.success-icon {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 18px;
  height: 18px;
  flex-shrink: 0;
  border-radius: 50%;
  background: var(--ion-color-success);
  color: #ffffff;
  font-size: 11px;
}

/* =========================================================
   CIPHER PROCESS
========================================================= */

.process-card {
  overflow: hidden;
}

.process-flow {
  display: flex;
  align-items: stretch;
  gap: 6px;
  width: 100%;
}

.process-step {
  display: flex;
  flex: 1;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-width: 0;
  min-height: 82px;
  padding: 10px 5px;
  border-radius: 11px;
  text-align: center;
}

.input-step {
  background: rgba(59, 130, 246, 0.1);
  color: #3b82f6;
}

.shift-step {
  background: rgba(245, 158, 11, 0.12);
  color: #d97706;
}

.output-step {
  background: rgba(22, 163, 74, 0.1);
  color: #16a34a;
}

.step-label {
  margin-bottom: 7px;
  font-size: 9px;
  font-weight: 850;
  letter-spacing: 0.3px;
  text-transform: uppercase;
}

.process-step strong {
  max-width: 100%;
  overflow-wrap: anywhere;
  color: inherit;
  font-size: 12px;
  line-height: 1.4;
}

.process-arrow {
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--ion-color-medium);
  font-size: 18px;
  font-weight: 850;
}

.process-description {
  display: flex;
  align-items: flex-start;
  gap: 7px;
  margin-top: 16px;
  color: var(--ion-color-medium);
  font-size: 11px;
  line-height: 1.6;
  text-align: left;
}

.description-icon {
  flex-shrink: 0;
  color: var(--ion-color-primary);
  font-size: 14px;
}

.letter-example {
  margin-top: 17px;
  padding: 14px;
  border: 1px dashed var(--ion-border-color);
  border-radius: 11px;
  background: var(--ion-background-color);
  text-align: center;
}

.example-label {
  margin: 0 0 9px;
  color: var(--ion-color-medium);
  font-size: 9px;
  font-weight: 850;
  letter-spacing: 1px;
}

.letter-flow {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 16px;
}

.letter-flow strong {
  color: var(--ion-color-primary);
  font-size: 21px;
}

.letter-flow span {
  color: var(--ion-color-medium);
  font-size: 18px;
}

.letter-example > p:last-child {
  margin: 8px 0 0;
  color: var(--ion-color-medium);
  font-size: 10px;
}

/* =========================================================
   DEVELOPER CREDIT
========================================================= */

.developer-credit {
  margin: 25px 0 10px;
  color: var(--ion-color-medium);
  font-size: 10px;
  line-height: 1.7;
  text-align: center;
}

.developer-credit strong {
  display: block;
  color: var(--ion-color-primary);
  font-size: 11px;
}

/* =========================================================
   ABOUT VIEW
========================================================= */

.about-container {
  padding-top: 22px;
}

.about-hero {
  padding-top: 10px;
}

.about-title {
  margin: 0 0 13px;
  color: var(--ion-text-color);
  font-size: 22px;
  font-weight: 850;
  text-align: center;
}

.about-description {
  margin: 0;
  color: var(--ion-color-medium);
  font-size: 13px;
  line-height: 1.8;
  text-align: center;
}

.formula-section {
  margin-top: 23px;
}

.formula-label {
  margin: 0 0 8px;
  color: var(--ion-color-medium);
  font-size: 9px;
  font-weight: 850;
  letter-spacing: 1px;
  text-align: center;
}

.formula-box {
  padding: 15px 10px;
  border: 1px solid rgba(124, 92, 232, 0.16);
  border-radius: 11px;
  background: rgba(124, 92, 232, 0.09);
  color: var(--ion-color-primary);
  font-size: 18px;
  font-weight: 850;
  letter-spacing: 0.4px;
  text-align: center;
}

.definitions-box {
  margin-top: 22px;
  padding: 13px;
  border: 1px solid var(--ion-border-color);
  border-radius: 11px;
}

.definition-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 15px;
  padding: 7px 0;
  color: var(--ion-color-medium);
  font-size: 11px;
}

.definition-row strong {
  min-width: 55px;
  color: var(--ion-color-primary);
  font-weight: 850;
}

.definition-row span {
  flex: 1;
  text-align: right;
}

.content-divider {
  height: 1px;
  margin: 24px 0;
  background: var(--ion-border-color);
}

.subsection-title {
  margin: 0 0 12px;
  color: var(--ion-text-color);
  font-size: 15px;
  font-weight: 850;
}

.example-box {
  padding: 13px;
  border: 1px solid var(--ion-border-color);
  border-radius: 11px;
  background: var(--ion-background-color);
}

.example-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 15px;
  padding: 8px 0;
  color: var(--ion-color-medium);
  font-size: 12px;
}

.example-row strong {
  color: var(--ion-text-color);
  font-weight: 850;
  letter-spacing: 0.7px;
}

.feature-list {
  margin: 0;
  padding-left: 19px;
  color: var(--ion-color-medium);
  font-size: 12px;
  line-height: 2;
}

.developer-label {
  margin: 0;
  color: var(--ion-color-medium);
  font-size: 9px;
  font-weight: 850;
  letter-spacing: 1px;
  text-align: center;
}

.developer-name {
  margin: 9px 0 5px;
  color: var(--ion-color-primary);
  font-size: 16px;
  font-weight: 850;
  text-align: center;
}

.developer-role {
  margin: 0;
  color: var(--ion-color-medium);
  font-size: 11px;
  line-height: 1.6;
  text-align: center;
}

.quote-box {
  margin: 22px 0;
  color: var(--ion-color-medium);
  font-size: 11px;
  font-style: italic;
  text-align: center;
}

/* =========================================================
   BOTTOM NAVIGATION
========================================================= */

.bottom-navigation {
  display: flex;
  justify-content: space-around;
  gap: 10px;
  padding: 8px 20px 10px;
  padding-bottom: calc(
    10px + env(safe-area-inset-bottom)
  );
}

.nav-button {
  display: flex;
  flex: 1;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 4px;
  min-height: 45px;
  border: none;
  border-radius: 11px;
  background: transparent;
  color: var(--ion-color-medium);
  font-size: 10px;
  font-weight: 700;
  transition: background 0.2s ease;
}

.nav-button.active {
  background: rgba(124, 92, 232, 0.1);
  color: var(--ion-color-primary);
}

.nav-icon {
  font-size: 20px;
  font-weight: 800;
}

/* =========================================================
   RESPONSIVE DESIGN
========================================================= */

@media (min-width: 600px) {
  .page-container,
  .landing-container {
    padding: 25px;
  }

  .hero-section {
    padding-top: 35px;
  }

  .hero-section h1 {
    font-size: 31px;
  }

  .app-card ion-card-content {
    padding: 25px;
  }

  .landing-container {
    max-width: 760px;
  }

  .landing-features {
    gap: 15px;
  }

  .landing-feature {
    padding: 20px 12px;
  }
}

@media (max-width: 380px) {
  .page-container,
  .landing-container {
    padding: 12px;
  }

  .app-card ion-card-content {
    padding: 16px;
  }

  .hero-section h1 {
    font-size: 24px;
  }

  .landing-hero h1 {
    font-size: 29px;
  }

  .landing-icon {
    width: 78px;
    height: 78px;
    font-size: 42px;
  }

  .landing-features {
    gap: 5px;
  }

  .landing-feature {
    padding: 12px 5px;
  }

  .landing-feature p {
    font-size: 8px;
  }

  .process-flow {
    gap: 3px;
  }

  .process-arrow {
    font-size: 14px;
  }

  .process-step {
    min-height: 75px;
    padding: 8px 3px;
  }

  .step-label {
    font-size: 8px;
  }

  .process-step strong {
    font-size: 10px;
  }

  .letter-flow {
    gap: 11px;
  }
}
</style>