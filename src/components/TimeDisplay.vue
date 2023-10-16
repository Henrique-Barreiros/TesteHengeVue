<script setup lang="ts">
import { defineProps } from 'vue';

interface TimeDisplayProps {
  datetime: string;
}

const props = defineProps<TimeDisplayProps>();

const datetimeObject = new Date(props.datetime);

const options: Intl.DateTimeFormatOptions = {
  hour: '2-digit',
  minute: '2-digit'
};

const time: string = datetimeObject.toLocaleTimeString('en-US-u-hc-h23', options);
</script>

<template>
  <span>{{ time }}</span>
</template>