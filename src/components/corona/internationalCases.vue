<template>
  <div>
    <div>국제 현황</div>
    <div class="buttons">
      <el-radio-group v-model="displayType" size="small">
        <el-radio-button
          v-for="(item, index) in displayTypes"
          :key="index"
          :label="index"
          >{{ item.alias }}</el-radio-button
        >
      </el-radio-group>
      <input type="text" v-model="keyword" />
    </div>
    <ul class="list">
      <li v-for="country in sortedCountries" :key="country.CountryCode">
        <img
          :src="require(`@/assets/flags/${country.CountryCode}.svg`)"
          :alt="country.Country"
        />
        <span class="name">{{ country.Country }}</span>
        <span class="cases"
          >{{ country.NewConfirmed }}/{{ country.TotalConfirmed }}</span
        >
      </li>
    </ul>
  </div>
</template>

<script>
//https://github.com/purecatamphetamine/country-flag-icons
import coronaMixin from "@/mixins/coronaMixin";
export default {
  name: "internationalCases",
  mixins: [coronaMixin],
  data() {
    return {
      keyword: "",
      countries: [],
      displayType: 0,
      displayTypes: [
        { alias: "Daily Worst", key: "NewConfirmed", compare: -1 },
        { alias: "Daily Best", key: "NewConfirmed", compare: 1 },
        { alias: "Daily Worst", key: "TotalConfirmed", compare: -1 },
        { alias: "Daily Best", key: "TotalConfirmed", compare: 1 },
      ],
    };
  },
  mounted() {
    this.fetchCases();
  },
  computed: {
    sortedCountries() {
      // sort
      const { compare, key } = this.displayTypes[this.displayType];
      let list = [...this.countries];
      // filter
      list = list.filter((li) =>
        li.Country.toUpperCase().includes(this.keyword.toUpperCase())
      );
      return list.sort((a, b) => (a[key] > b[key] ? compare : -compare));
    },
  },
  methods: {
    async fetchCases() {
      const url = "https://api.covid19api.com/summary";
      const summary = await this.fetchData("get", url);
      console.log(summary, "res");
      this.countries = summary.Countries;
    },
  },
};
</script>

<style scoped>
.buttons {
  margin-top: 1em;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.list {
  list-style: none;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 0.5em;
  margin-top: 1em;
}
.list li {
  border: 1px solid #efefef;
  display: flex;
  justify-content: flex-start;
  align-items: center;
  padding: 0.5em;
  font-size: 12px;
}
.list img {
  max-width: 1em;
  margin-right: 1em;
}
.list .name {
  font-weight: bold;
  margin-right: 1em;
}
</style>
