<template>
  <div class="bg-gray-100 p-6 rounded-lg shadow-md h-44">
    <div class="flex justify-between items-center">
      <h3 class="text-lg font-bold">{{ type || "Carte de Crédit" }}</h3>
      <img v-if="logo" :src="logo" alt="Logo" class="h-6" />
    </div>
    <p class="text-xl font-mono mt-4">{{ displayCardNumber }}</p>
    <div class="flex justify-between mt-2">
      <p>{{ cardHolder || "NOM DU TITULAIRE" }}</p>
      <p>{{ expiryDate || "MM/YY" }}</p>
    </div>
  </div>
</template>

<script>
import visaLogo from "@/assets/images/visa.svg";
import mastercardLogo from "@/assets/images/mastercard.svg";

export default {
  props: ["cardNumber", "cardHolder", "expiryDate"],
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
