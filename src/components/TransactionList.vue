<template>
  <h3>History</h3>
  
  <ul id="list" class="list">
    <li style="text-align: center;" v-if="!transactions.length">No transactions yet</li>
    <li
      v-if="transactions.length"
      v-for="t in transactions"
      :class="{ plus: t.amount > 0, minus: t.amount < 0 }"
    >
      {{ t.text }} <span>{{ t.amount }}</span
      ><button @click="removeTransaction(t.id)" class="delete-btn">x</button>
    </li>
  </ul>
</template>
<script setup>
const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});
const emit = defineEmits(["removeTransaction"]);

const removeTransaction = (id) => {
  console.log("removeTransaction", id)
  emit("removeTransaction", id);
};
</script>
<style></style>
