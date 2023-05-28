
<script>
export default {
  data() {
    return {
      // range: ["50000 to 70000", "70000 to 10000", "greater than 100000"],
      colors: ['red', 'blue', 'yellow'], 
      range: [
        {id: "price_range_none", from: 50000, to: 70000, text: "Select price range"}, 
        {id: "price_range_a", from: 50000, to: 70000, text: "50000 to 70000"}, 
        {id: "price_range_b", from: 70000, to: 90000, text: "70000 to 90000"},
        {id: "price_range_c", from: 90000, to: 500000, text: "greater than 90000"}
      ],
      colorSelected: '',
      priceRangeSelected: '',
      // priceRangeSelected: {},
      // color2: '',  
    }
  }, 
  methods: {
    applyFilter() {
      let data = {
        color: this.colorSelected,
        priceRange: this.priceRangeSelected
      }
      this.$emit('apply-filter', data);
    }
  }
}
</script>



<template>

<!-- Modal -->
<div class="modal fade" id="filterModal" tabindex="-1" aria-labelledby="filterModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h1 class="modal-title fs-5" id="filterModalLabel">Filter</h1>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>

      <div class="modal-body">
        <div class="mb-3">
          <!-- <label for="" class="form-label">Price range</label> -->
          <div class="mb-3">
            <label for="" class="form-label">Price range</label>
            <select class="form-select" aria-label="Select price rangee"
              v-model="priceRangeSelected"
            >
              <!-- <option selected v-bind:value="priceRangeSelected">{{ priceRangeSelected.text }}</option> -->
              <option value="">No range </option>
              <option v-bind:value="range[1]">{{ range[1].text }}</option>
              <option v-bind:value="range[2]">{{ range[2].text }}</option>
              <option v-bind:value="range[3]">{{ range[3].text }}</option>
            </select>
          </div>
        </div>
        <div class="mb-3">
          <label for="" class="form-label">Color</label>
          <select class="form-select" 
            v-model="colorSelected"
          >
            <option value="">No color</option>
            <option v-bind:value="colors[0]">{{ colors[0]}}</option>
            <option v-bind:value="colors[1]">{{ colors[1]}}</option>
            <option v-bind:value="colors[2]">{{ colors[2]}}</option>
          </select>
        </div>
      </div>

      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary"
          @click="applyFilter()"
        >Apply filter</button>
      </div>
    </div>
  </div>
</div>

</template>
