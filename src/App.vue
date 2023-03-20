<template>
  <EasyDataTable v-model:items-selected="itemsSelected" show-index :headers="headers" :items="items"
    :filter-options="filterOptions">
    <template #header-age="header">
      <div class="filter-column">
        <img src="./eglass-filter.png" class="filter-icon" @click="showAgeFilter = !showAgeFilter" />
        {{ header.text }}
        <div class="filter-menu filter-age-menu" v-if="showAgeFilter">
          <Slider v-model="ageCriteria" class="slider" />
        </div>
      </div>
    </template>
    <template #header-favouriteSport="header">
      <div class="filter-column">
        <img src="./eglass-filter.png" class="filter-icon"
          @click="showFavouriteSportFilter = !showFavouriteSportFilter" />
        {{ header.text }}
        <div class="filter-menu filter-sport-menu" v-if="showFavouriteSportFilter">
          <select class="favouriteSport-selector" v-model="favouriteSportCriteria" name="favouriteSport">
            <option value="swimming">swimming</option>
            <option value="basketball">basketball</option>
            <option value="running">running</option>
            <option value="football">football</option>
            <option value="all">all</option>
          </select>
        </div>
      </div>
    </template>
  </EasyDataTable>
</template>

<script>
import Slider from "@vueform/slider";
import { ref, computed } from "vue";
import { mockClientItems } from "./mock";
import "@vueform/slider/themes/default.css";

export default {
  components: { Slider },
  setup() {
    const ageCriteria = ref([1, 15]);
    const favouriteSportCriteria = ref("swimming");
    const filterOptions = computed(() => {
      const filterOptionsArray = [];
      if (favouriteSportCriteria.value !== "all") {
        filterOptionsArray.push({
          field: "favouriteSport",
          comparison: "=",
          criteria: favouriteSportCriteria.value,
        });
      }
      filterOptionsArray.push({
        field: "age",
        comparison: "between",
        criteria: ageCriteria.value,
      });
      return filterOptionsArray;
    });

    const items = ref(mockClientItems(100));
    const headers = [
      { text: "Name", value: "name" },
      { text: "Address", value: "address" },
      { text: "Height", value: "height", sortable: true },
      { text: "Weight", value: "weight", sortable: true },
      { text: "Age", value: "age", sortable: true },
      { text: "Favourite sport", value: "favouriteSport" },
      { text: "Favourite fruits", value: "favouriteFruits" },
    ];
    const itemsSelected = ref([items.value[14]]);

    const showAgeFilter = ref(false);
    const showFavouriteSportFilter = ref(false);

    return {
      showAgeFilter,
      showFavouriteSportFilter,
      ageCriteria,
      favouriteSportCriteria,
      filterOptions,
      headers,
      items,
      itemsSelected,
    };
  },
};
</script>

<style>
.filter-column {
  display: flex;
  align-items: center;
  justify-items: center;
  position: relative;
}

.filter-icon {
  cursor: pointer;
  display: inline-block;
  width: 15px !important;
  height: 15px !important;
  margin-right: 4px;
}

.filter-menu {
  padding: 15px 30px;
  z-index: 1;
  position: absolute;
  top: 30px;
  width: 200px;
  background-color: #fff;
  border: 1px solid #e0e0e0;
}

.filter-age-menu {
  height: 40px;
}

.slider {
  margin-top: 36px;
}

.favouriteSport-selector {
  width: 100%;
}
</style>
