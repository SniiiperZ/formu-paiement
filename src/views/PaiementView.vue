<script>
import Card from "../components/FormPaiement.vue";
import { ref } from "vue";
const contactData = ref({
  cardNumber: "",
  cardHolder: "",
  expiryMonth: "",
  expiryYear: "",
  cvv: "",
  cardNumberError: "",
  expiryDateError: "",
  cvvError: "",
  cardType: "",
});

export default {
  components: {
    Card,
  },
  setup() {
    return {
      contactData,
    };
  },
  computed: {
    formHasErrors() {
      return (
        contactData.value.cardNumberError ||
        contactData.value.expiryDateError ||
        contactData.value.cvvError
      );
    },
  },
  methods: {
    submitForm() {
      console.log(
        "Formulaire soumis",
        contactData.value.cardNumber,
        contactData.value.expiryMonth,
        contactData.value.expiryYear,
        contactData.value.cvv
      );
    },
    validateCardNumber() {
      contactData.value.cardNumber,
        contactData.value.cardHolder,
        contactData.value.expiryMonth,
        contactData.value.expiryYear,
        contactData.value.cvv;
      this.detectCardType();
    },
    validateExpiryDate() {
      if (!this.luhnCheck(contactData.value.cardNumber)) {
        contactData.value.cardNumberError = "Numéro de carte invalide";
      } else {
        contactData.value.cardNumberError = "";
      }
      contactData.value.expiryDateError = "";
    },
    validateCVV() {
      const expiry = new Date(
        contactData.value.expiryYear,
        contactData.value.expiryMonth - 1
      );
      if (expiry < new Date()) {
        contactData.value.expiryDateError = "Date d'expiration dépassée";
      } else {
        contactData.value.expiryDateError = "";
      }
    },
    luhnCheck(value) {
      let sum = 0;
      let shouldDouble = false;
      for (let i = value.length - 1; i >= 0; i--) {
        let digit = parseInt(value.charAt(i));
        if (shouldDouble) {
          digit *= 2;
          if (digit > 9) {
            digit -= 9;
          }
        }
        sum += digit;
        shouldDouble = !shouldDouble;
      }
      return sum % 10 === 0;
    },
    detectCardType() {
      const visaRegex = /^4[0-9]{12}(?:[0-9]{3})?$/;
      const masterCardRegex = /^5[1-5][0-9]{14}$/;
      if (visaRegex.test(contactData.value.cardNumber)) {
        contactData.value.cardType = "Visa";
      } else if (masterCardRegex.test(contactData.value.cardNumber)) {
        contactData.value.cardType = "Mastercard";
      } else {
        contactData.value.cardType = "";
      }
    },
  },
};
</script>

<template>
  <Card
    :cardNumber="contactData.cardNumber"
    :cardHolder="contactData.cardHolder"
    :expiryMonth="contactData.expiryMonth"
    :expiryYear="contactData.expiryYear"
    :cardType="contactData.cardType"
  />
  <form @submit.prevent="submitForm"></form>
</template>

<style scoped>
span {
  color: red;
}
</style>
