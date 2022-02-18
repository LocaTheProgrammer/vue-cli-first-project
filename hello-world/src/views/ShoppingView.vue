<template>

    <Shopping/>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import Shopping from '@/components/Shopping.vue'; // @ is an alias to /src

@Options({
  components: {
    Shopping,
  },
})
export default class ShoppingView extends Vue {}
</script>
