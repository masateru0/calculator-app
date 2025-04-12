<template>
    <div class="calculator">
        <Display :value="display" />
        <ButtonPanel @buttonClick="handleButtonClick" />
    </div>
</template>

<script setup></script>