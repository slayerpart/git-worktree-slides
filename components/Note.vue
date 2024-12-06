<template>
    <div class="note">
        <strong>Note:</strong>
        <slot />
    </div>
</template>

<style scoped>
.note {
    border: 0.5px solid light-dark(#333, #eaeaea);
    border-radius: 5px;
    max-width: max-content;
    padding: 3px 12px;
    @apply text-sm;
}
.note > strong {
    margin-right: 0.5rem;
}
</style>