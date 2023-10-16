<script setup lang="ts">
import { defineProps, computed, ref } from 'vue';

interface RecipientsDisplayProps {
  recipients: string[];
  availableWidth: number;
  cellWidth: number;
}

const props = defineProps<RecipientsDisplayProps>();

const totalTruncatedRecipients = ref(0);
const isTruncated = ref(false);
const displayedRecipients = computed(() => {
  let totalWidth = 0;
  const trimmedRecipients: string[] = [];
  const badgeWidth = 30;

  props.recipients.forEach((recipient) => {
    const recipientWidth = recipient.length * 10;
    if (totalWidth + recipientWidth + badgeWidth < props.availableWidth) {
      trimmedRecipients.push(recipient);
      totalWidth += recipientWidth;
    } else {
      totalTruncatedRecipients.value += 1;
    }
  });

  isTruncated.value = totalTruncatedRecipients.value > 0;

  return trimmedRecipients;
});
</script>

<template>
  <div class="recipients-display">
    <div class="recipients-list">
      <span v-for="(recipient, index) in displayedRecipients" :key="index">
        {{ recipient }}
        <span v-if="index === displayedRecipients.length - 1 && isTruncated">
          , <span class="ellipsis">...</span>
          <span class="badge" v-if="totalTruncatedRecipients > 0">+{{ totalTruncatedRecipients }}</span>
        </span>
        <span v-else-if="index < displayedRecipients.length - 1">, </span>
      </span>
    </div>
  </div>
</template>
  
  <style scoped>
  .recipients-display {
    display: flex;
    flex-direction: column;
  }
  
  .recipients-list {
    display: flex;
  }
  
  .ellipsis {
    font-size: 16px;
    color: #333333;
  }
  
  .badge {
    font-size: 16px;
    color: #f0f0f0;
    background-color: #666666;
    border-radius: 3px;
    padding-top: 2px;
    padding-bottom: 2px;
    padding-left: 5px;
    padding-right: 5px;
    margin-left: 5px;
  }
  </style>
  