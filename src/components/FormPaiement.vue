<template>
  <div class="max-w-md mx-auto mt-10 p-6 bg-white rounded shadow-lg">
    <!-- Affichage dynamique de la carte -->
    <CreditCardDisplay
      :cardNumber="form.cardNumber"
      :cardHolder="form.cardHolder"
      :expiryDate="form.expiryDate"
      :cvv="form.cvv"
      :isFlipped="isCvvActive"
    />

    <form @submit.prevent="handleSubmit" class="space-y-4 mt-6">
      <div>
        <label for="cardNumber" class="block text-sm font-medium text-gray-700"
          >Numéro de carte</label
        >
        <input
          type="text"
          id="cardNumber"
          v-model="form.cardNumber"
          @input="validateCardNumber"
          class="mt-1 block w-full p-2 border rounded-md"
          placeholder="1234 5678 9012 3456"
        />
        <p v-if="errors.cardNumber" class="text-red-500 text-sm">
          {{ errors.cardNumber }}
        </p>
      </div>
      <div>
        <label for="cardHolder" class="block text-sm font-medium text-gray-700"
          >Nom du titulaire</label
        >
        <input
          type="text"
          id="cardHolder"
          v-model="form.cardHolder"
          class="mt-1 block w-full p-2 border rounded-md"
          placeholder="John Doe"
        />
      </div>
      <div class="flex space-x-4">
        <div>
          <label
            for="expiryDate"
            class="block text-sm font-medium text-gray-700"
            >Date d'expiration</label
          >
          <input
            type="text"
            id="expiryDate"
            v-model="form.expiryDate"
            @input="validateExpiryDate"
            class="mt-1 block w-full p-2 border rounded-md"
            placeholder="MM/YY"
          />
          <p v-if="errors.expiryDate" class="text-red-500 text-sm">
            {{ errors.expiryDate }}
          </p>
        </div>
        <div>
          <label for="cvv" class="block text-sm font-medium text-gray-700"
            >Code CVV</label
          >
          <input
            type="text"
            id="cvv"
            v-model="form.cvv"
            @focus="isCvvActive = true"
            @blur="isCvvActive = false"
            @input="validateCVV"
            class="mt-1 block w-full p-2 border rounded-md"
            placeholder="123"
          />
          <p v-if="errors.cvv" class="text-red-500 text-sm">{{ errors.cvv }}</p>
        </div>
      </div>
      <button
        type="submit"
        class="w-full bg-blue-500 text-white py-2 px-4 rounded hover:bg-blue-600"
      >
        Soumettre
      </button>
    </form>
  </div>
</template>

<script>
import CreditCardDisplay from "./CreditCardDisplay.vue";

export default {
  components: {
    CreditCardDisplay,
  },
  data() {
    return {
      isCvvActive: false, // État pour contrôler le retournement de la carte
      form: {
        cardNumber: "",
        cardHolder: "",
        expiryDate: "",
        cvv: "",
      },
      errors: {
        cardNumber: null,
        expiryDate: null,
        cvv: null,
      },
    };
  },
  methods: {
    validateCardNumber() {
      const luhnCheck = (number) => {
        let sum = 0;
        let shouldDouble = false;
        for (let i = number.length - 1; i >= 0; i--) {
          let digit = parseInt(number[i], 10);
          if (shouldDouble) {
            digit *= 2;
            if (digit > 9) digit -= 9;
          }
          sum += digit;
          shouldDouble = !shouldDouble;
        }
        return sum % 10 === 0;
      };

      const cleanNumber = this.form.cardNumber.replace(/\s+/g, "");
      if (!luhnCheck(cleanNumber)) {
        this.errors.cardNumber = "Numéro de carte invalide.";
      } else {
        this.errors.cardNumber = null;
      }
    },
    validateExpiryDate() {
      const [month, year] = this.form.expiryDate.split("/");
      const now = new Date();
      const expiry = new Date(`20${year}`, month - 1);
      if (expiry < now || !month || !year || month < 1 || month > 12) {
        this.errors.expiryDate = "Date d’expiration invalide.";
      } else {
        this.errors.expiryDate = null;
      }
    },
    validateCVV() {
      const isValid = /^\d{3,4}$/.test(this.form.cvv);
      this.errors.cvv = isValid ? null : "Code CVV invalide.";
    },
    handleSubmit() {
      if (
        !this.errors.cardNumber &&
        !this.errors.expiryDate &&
        !this.errors.cvv
      ) {
        alert("Formulaire soumis avec succès !");
      } else {
        alert("Veuillez corriger les erreurs avant de soumettre.");
      }
    },
  },
};
</script>
