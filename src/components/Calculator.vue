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
      payPeriods: ['Monthly', 'Annualy'],
      selectedPayPeriod: 'Monthly',
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
          ageRangeTaxThresholds: [75000, 116150, 129850],
        },
        {
          year: 2018,
          rateTax: [18, 26, 31, 36, 39, 41, 45],
          taxableAmnt: [0, 34178, 61910, 97225, 149475, 209032, 533625],
          incomeBase: [0, 189880, 296540, 410460, 555600, 708310, 1500000],
          ageRangeTaxRebate: [13635, 7479, 2493],
          ageRangeTaxThresholds: [75750, 117300, 131150],
        },
      ],
    };
  },
  methods: {
    calculateTax() {
      let yearIndex = this.getYearIndex(this.selectedYear);
      let inputSalary = Number(this.salary);
      if (this.selectedPayPeriod == 'Monthly') {
        inputSalary = inputSalary * 12;
      }
      let tax = this.taxBracket(inputSalary, yearIndex);
      let rebate = this.calculateRebate(Number(this.age), yearIndex);

      let total = (tax - rebate) / 12;
      alert('The total is ' + total);
    },
    taxBracket(salary, year) {
      let taxRate = 0;
      let taxableAmnt = 0;
      let incomeBase = 0;

      let isFound = false;
      let index = 0;
      let taxYear = this.yearData[year];
      let length = taxYear.rateTax.length;

      while (!isFound) {
        if (salary < taxYear.incomeBase[index + 1]) {
          taxRate = taxYear.rateTax[index];
          taxableAmnt = taxYear.taxableAmnt[index];
          incomeBase = taxYear.incomeBase[index];
          isFound = true;
        }

        index += 1;
        let length = taxYear.rateTax.length;

        if (index > length) {
          taxRate = taxYear.rateTax[length - 1];
          taxableAmnt = taxYear.taxableAmnt[length - 1];
          incomeBase = taxYear.incomeBase[length - 1];
          isFound = true;
        }
      }

      return (salary - incomeBase) * (taxRate / 100) + taxableAmnt;
    },
    getYearIndex(year) {
      let index = 0;
      this.yearData.forEach((data, i) => {
        if (data.year === year) {
          index = i;
        }
      });

      return index;
    },
    calculateRebate(age, year) {
      let index = 0;
      let taxRebateAmnt = 0;

      const AGE_CLASS_ONE = 65;
      const AGE_CLASS_TWO = 75;

      if (age < AGE_CLASS_ONE) {
        index = 0;
      } else if (age > AGE_CLASS_ONE && age < AGE_CLASS_TWO) {
        index = 1;
      } else if (age > AGE_CLASS_TWO) {
        index = 2;
      }

      taxRebateAmnt = this.yearData[year].ageRangeTaxRebate[index];

      if (this.yearData[year].ageRangeTaxRebate[index - 1]) {
        taxRebateAmnt += this.yearData[year].ageRangeTaxRebate[index - 1];
      }

      return taxRebateAmnt;
    },
  },
};
</script>

<style>

</style>
