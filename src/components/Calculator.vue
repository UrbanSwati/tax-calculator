<template>
  <div>
    <div class="row">
      <div class="col-md-6 offset-md-3">
        <form>

          <div class="form-row">
            <label class="col-sm-8 col-form-label">Which tax year would you like to calculate?</label>
            <div class="col-sm-4">
              <select class="form-control form-control-sm" v-model="selectedYear">
                <option v-for="year in yearData" :key="year.year" :value="year.year">{{ year.year }}</option>
              </select>
            </div>
          </div>

          <div class="form-row">
            <label class="col-sm-8 col-form-label">What is your total salary before deductions? </label>
            <div class="col-sm-4">
              <div class="input-group-prepend">
                <div class="input-group-text">R</div>
              <input type="number" class="form-control" id="inlineFormInputGroup" v-model="salary" placeholder="Salary">
              </div>
            </div>
          </div>

          <div class="form-row">
            <label class="col-sm-8 col-form-label">How often do you receive this salary? </label>
            <div class="col-sm-4">
              <select class="form-control form-control-sm" v-model="selectedPayPeriod">
                <option v-for="period in payPeriods" :key="period" :value="period" >{{ period }}</option>
              </select>
            </div>
          </div>

         <div class="form-row">
            <label class="col-sm-8 col-form-label">Your age?</label>
            <div class="col-sm-4">
              <input type="number" class="form-control" id="inlineFormInputGroup" v-model="age" placeholder="age">
            </div>
          </div>

           <div class="form-row">
            <label class="col-sm-8 col-form-label">Number of medical aid dependent(s):</label>
            <div class="col-sm-4">
              <input type="number" class="form-control" id="inlineFormInputGroup" v-model="dependents" placeholder="No. of Dependents">
            </div>
          </div>
  <br>
           <div class="form-row">
             <div class="col">
             <button class="btn btn-primary btn-block" @click.prevent="calculateTax">Submit</button>
             </div>
           </div>

        </form>

      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedYear: 2017,
      payPeriods: ["Monthly", "Annualy"],
      selectedPayPeriod: "Monthly",
      salary: 0,
      age: 18,
      dependents: 0,
      yearData: [
        {
          year: 2017,
          rateTax: [18, 26, 31, 36, 39, 41],
          taxableAmnt: [0, 33840, 61296, 96264, 147996, 206964],
          incomeBase: [0, 188000, 293600, 406400, 550100, 701300],
          ageRangeTaxRebate: [13500, 7407, 2466],
          ageRangeTaxThresholds: [75000, 116150, 129850]
        }
      ]
    };
  },
  methods: {
    calculateTax() {
      console.log("button clicked");
      let tax = this.taxBracket(Number(this.salary), this.getYearIndex(this.selectedYear));
      console.log('answer: ', tax);
      
    },
    taxBracket(salary, year) {
      let taxRate = 0;
      let taxableAmnt = 0;
      let incomeBase = 0;

      if (salary < this.yearData[year].incomeBase[1]) {
        taxRate = this.yearData[year].rateTax[0];
        taxableAmnt = this.yearData[year].taxableAmnt[0];
        incomeBase = this.yearData[year].incomeBase[0];
      } else if (salary < this.yearData[year].incomeBase[2]) {
        taxRate = this.yearData[year].rateTax[1];
        taxableAmnt = this.yearData[year].taxableAmnt[1];
        incomeBase = this.yearData[year].incomeBase[1];
      } else if (salary < this.yearData[year].incomeBase[3]) {
        taxRate = this.yearData[year].rateTax[2];
        taxableAmnt = this.yearData[year].taxableAmnt[2];
        incomeBase = this.yearData[year].incomeBase[2];
      } else if (salary < this.yearData[year].incomeBase[4]) {
        taxRate = this.yearData[year].rateTax[3];
        taxableAmnt = this.yearData[year].taxableAmnt[3];
        incomeBase = this.yearData[year].incomeBase[3];
      } else if (salary < this.yearData[year].incomeBase[5]) {
        taxRate = this.yearData[year].rateTax[4];
        taxableAmnt = this.yearData[year].taxableAmnt[4];
        incomeBase = this.yearData[year].incomeBase[4];
      } else {
        taxRate = this.yearData[year].rateTax[5];
        taxableAmnt = this.yearData[year].taxableAmnt[5];
        incomeBase = this.yearData[year].incomeBase[5];
      }

      return (salary - incomeBase) * (taxRate / 100) + taxableAmnt;
    },
    getYearIndex(year) {
      let index = 0;

      for (let i = 0; i < this.yearData.length; i++) {
        const element = this.yearData[i];
        if (element.year == year) {
          index = i;
          break;
        }
      }
      return index;
    }
  }
};
</script>

<style>

</style>
