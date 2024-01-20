<template>
  <h3>Add new transaction</h3>
  <form @submit.prevent="handleSubmit" id="form">
    <div class="form-control">
      <label for="text">Text</label>
      <input v-model="text" type="text" id="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        v-model="amount"
        type="text"
        id="amount"
        placeholder="Enter amount..."
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>
<script setup>
import { ref } from "vue";
import { useToast } from "vue-toastification";

const text = ref("");
const amount = ref("");
const toast = useToast();

const emit = defineEmits(["submitted"]);

const handleSubmit = () => {
  if (!text.value || !amount.value) {
    toast.error("Both fields are required");
    return;
  }

  const formData = {
    text: text.value,
    amount: parseFloat(amount.value),
  };

  text.value = "";
  amount.value = ""

  emit("submitted", formData);
};
</script>
<style></style>
