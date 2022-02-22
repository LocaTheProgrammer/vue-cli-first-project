<template>
    <LoginRest/>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import LoginRest from '@/components/LoginRest.vue'; // @ is an alias to /src

@Options({
  components: {
    LoginRest,
  },
})
export default class LoginRestView extends Vue {}
</script>
