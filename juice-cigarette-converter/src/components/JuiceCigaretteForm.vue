<style lang="scss" src="./JuiceCigarette.scss" scoped />
<template>
  <q-page class="row items-center justify-evenly">
    <q-form @submit="onSubmit" @reset="onReset">
      <div class="columns">
        <div class="left-col">
          <img src="~assets/juice.svg" />
          <q-input
            style="width: 200px"
            required
            v-model="nicotineContentMg"
            label="Nicotine content (mg)"
            type="number"
          >
          </q-input>
          <q-input
            style="width: 200px"
            required
            v-model="juiceSizeMl"
            label="Juice bottle size (ml)"
            type="number"
          />
        </div>
        <div class="right-col">
          <img src="~assets/cigarette.svg" />
          <q-select
            style="width: 200px"
            required
            v-model="selectedCigarette"
            :options="cigaretteList"
            label="Select cigarette"
            @update:model-value="nicotinePerCigarette = $event.nicotineContent"
            option-label="name"
          >
          </q-select>
          <q-input
            style="width: 200px"
            :disable="
              selectedCigarette && selectedCigarette.name === 'Other'
                ? false
                : true
            "
            required
            v-model="nicotinePerCigarette"
            label="Nicotine per cigarette (mg)"
            type="number"
          />
        </div>
      </div>
      <div class="action-buttons">
        <q-btn type="submit" color="primary" label="Results" />
        <q-btn type="reset" color="primary" label="Reset" />
      </div>
      <div class="results">
        <p>Total nicote juice: {{ totalNicotineJuiceMg }}</p>
        <p>Total nicote pack: {{ totalNicotinePackMg }}</p>
        <p>Total cigarettes: {{ totalCigarettes }}</p>
        <p>Total packs: {{ totalPacks }}</p>
      </div>
    </q-form>
  </q-page>
</template>
<script lang="ts">
import { ref } from 'vue';
import { Cigarette } from './models';

export default {
  name: 'JuiceCigaretteForm',
  setup() {
    const nicotineContentMg = ref(0);
    const juiceSizeMl = ref(0);

    const selectedCigarette: Cigarette | any = ref(null);
    const nicotinePerCigarette = ref(0);
    const cigaretteList = [
      { name: 'Marlboro Gold', nicotineContent: 0.6 },
      { name: 'Marlboro Red', nicotineContent: 0.8 },
      { name: 'Other', nicotineContent: null },
    ];

    let totalNicotineJuiceMg = ref(0);
    let totalNicotinePackMg = ref(0);
    let totalCigarettes = ref('');
    let totalPacks = ref('');

    const onSubmit = () => {
      if (nicotineContentMg.value !== 0 && juiceSizeMl.value !== 0) {
        totalNicotineJuiceMg.value = nicotineContentMg.value * juiceSizeMl.value;
        totalNicotinePackMg.value = selectedCigarette.value.nicotineContent * 20;
        totalCigarettes.value = (totalNicotineJuiceMg.value / selectedCigarette.value.nicotineContent).toFixed(1);
        totalPacks.value = (totalNicotineJuiceMg.value / totalNicotinePackMg.value).toFixed(1);
      }
    };

    const onReset = () => {
      nicotineContentMg.value = 0;
      juiceSizeMl.value = 0;
      selectedCigarette.value = null;
      nicotinePerCigarette.value = 0;
    };

    return {
      nicotineContentMg,
      juiceSizeMl,
      selectedCigarette,
      nicotinePerCigarette,
      cigaretteList,
      totalNicotineJuiceMg,
      totalNicotinePackMg,
      totalCigarettes,
      totalPacks,
      onSubmit,
      onReset,
    };
  },
};
</script>