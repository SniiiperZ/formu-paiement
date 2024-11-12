<template>
  <div class="relative bg-gray-100 w-96 h-56 rounded-lg shadow-lg perspective">
    <!-- Conteneur avec effet de retournement -->
    <div
      class="credit-card-inner transform transition-transform duration-700 w-full h-full"
      :class="{ 'rotate-y-180': isFlipped }"
    >
      <!-- Face avant -->
      <div
        class="credit-card-front absolute w-full h-full bg-gradient-to-br from-blue-500 via-blue-600 to-blue-800 rounded-lg backface-hidden p-6"
      >
        <div class="flex justify-between items-center">
          <h3 class="text-lg font-semibold tracking-wider text-white">
            {{ type || "Carte de Crédit" }}
          </h3>
          <img v-if="logo" :src="logo" alt="Logo" class="h-8" />
        </div>
        <p class="text-2xl font-mono mt-8 tracking-widest text-gray-200">
          {{ displayCardNumber }}
        </p>
        <div class="flex justify-between items-center mt-6">
          <p class="text-gray-300 text-sm uppercase">
            {{ cardHolder || "Nom du titulaire" }}
          </p>
          <p class="text-gray-300 text-sm">{{ expiryDate || "MM/YY" }}</p>
        </div>
      </div>

      <!-- Face arrière -->
      <div
        class="credit-card-back absolute w-full h-full bg-gray-900 rounded-lg backface-hidden transform rotate-y-180 p-6"
      >
        <!-- Bande magnétique -->
        <div class="bg-gray-800 h-10 w-full mb-4"></div>
        <!-- Section CVV -->
        <div class="bg-gray-700 h-10 w-full flex items-center justify-end px-4">
          <span class="text-white font-mono text-sm">{{ cvv || "•••" }}</span>
        </div>
        <p class="text-gray-400 text-xs mt-4">Code CVV</p>
      </div>
    </div>
  </div>
</template>

<script>
import visaLogo from "@/assets/images/visa.svg";
import mastercardLogo from "@/assets/images/mastercard.svg";

export default {
  props: ["cardNumber", "cardHolder", "expiryDate", "cvv", "isFlipped"],
  computed: {
    type() {
      const number = this.cardNumber || "";
      if (/^4/.test(number)) return "Visa";
      if (/^5[1-5]/.test(number)) return "MasterCard";
      return null;
    },
    logo() {
      return this.type === "Visa"
        ? visaLogo
        : this.type === "MasterCard"
        ? mastercardLogo
        : null;
    },
    displayCardNumber() {
      return this.cardNumber
        ? this.cardNumber.replace(/\d{4}(?=.)/g, "$& ")
        : "•••• •••• •••• ••••";
    },
  },
};
</script>

<style scoped>
/* Perspective pour un effet 3D */
.perspective {
  perspective: 1500px;
}

/* Conteneur interne gérant la rotation */
.credit-card-inner {
  transform-style: preserve-3d;
  transition: transform 0.6s ease-in-out;
  width: 100%;
  height: 100%;
}

/* Ajout de la classe de rotation */
.rotate-y-180 {
  transform: rotateY(180deg);
}

/* Styles pour les faces avant et arrière */
.credit-card-front,
.credit-card-back {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden; /* Cache la face non visible */
  border-radius: 0.75rem;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1), 0 1px 3px rgba(0, 0, 0, 0.06);
}

/* Rotation de la face arrière */
.credit-card-back {
  transform: rotateY(180deg);
}
</style>
