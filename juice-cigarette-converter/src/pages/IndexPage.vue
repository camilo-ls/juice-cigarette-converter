<template>
  <q-page class="row items-center justify-evenly">
    <juice-cigarette-form-vue />
  </q-page>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import JuiceCigaretteFormVue from 'src/components/JuiceCigaretteForm.vue';

export default defineComponent({
  name: 'IndexPage',
  components: { JuiceCigaretteFormVue },
});
</script>
