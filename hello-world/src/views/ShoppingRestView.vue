<template>

    <ShoppingRest/>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import ShoppingRest from '@/components/ShoppingRest.vue'; // @ is an alias to /src

@Options({
  components: {
    ShoppingRest,
  },
})
export default class ShoppingRestView extends Vue {}
</script>
