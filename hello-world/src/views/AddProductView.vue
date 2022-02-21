<template>

    <AddProduct/>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import AddProduct from '@/components/AddProduct.vue'; // @ is an alias to /src

@Options({
  components: {
    AddProduct,
  },
})
export default class AddProductView extends Vue {}
</script>
