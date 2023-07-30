<template>
    <div class="bg-gradient-to-r from-blue-300 to-purple-400 min-h-screen flex items-center justify-center">
        <Head title="Paypal fee calculator" />

        <div class="max-w-lg bg-white rounded-lg shadow-lg p-6 mx-8">
            <h1 class="text-3xl font-bold mb-4 text-center">PayPal Calculator App (UK Only)</h1>
            <div>
                <h2 class="text-xl font-bold mb-4">Buyer Sends You (in GBP)</h2>
                <input v-model="buyerAmount" type="number" class="border p-2 rounded-md w-full" />
                <div v-if="buyerAmount >= 0">
                    <div class="mt-4">
                        <strong>You Receive:</strong> <span class="text-green-800 font-bold">£{{ calculateTotalToReceive() }}</span>
                    </div>
                </div>
            </div>
            <div class="mt-3 bg-gray-100 p-6 rounded-lg">
                Standard rate for receiving domestic transactions with PayPal
                <br>
                2.9% of the transaction value + fixed fee of 0.30p
            </div>
        </div>
    </div>
</template>

<script setup>
import { Head } from "@inertiajs/vue3";
import { ref, computed } from 'vue';

// PayPal fees for transactions within the UK
const sellerFeePercentage = 2.9;
const sellerFeeFixed = 0.3;

// Create a reactive variable for the buyer's amount
const buyerAmount = ref(null);

// Calculate total amount for the buyer to pay (no fees for buyers within the UK)
const calculateTotalToPay = () => {
    return (buyerAmount.value || 0).toFixed(2);
};

// Calculate total amount for the seller to receive after deducting fees
const calculateTotalToReceive = () => {
    const amount = buyerAmount.value || 0;
    const fee = (amount * sellerFeePercentage / 100) + sellerFeeFixed;
    const totalToReceive = amount - fee;
    return totalToReceive.toFixed(2);
};
</script>

<style>
/* Custom styles (Tailwind CSS classes) */
.bg-gray-50 {
    background-color: #f9fafb;
}

.container {
    max-width: 600px;
}

input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
    appearance: none;
    margin: 0;
}

input[type="number"] {
    -moz-appearance: textfield;
}
</style>
