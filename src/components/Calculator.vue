<template>
  <div>
    <div class="row">
      <div class="col-md-6 offset-md-3">
        <form>

          <div class="form-row">
            <label class="col-sm-8 col-form-label">Which tax year would you like to calculate?</label>
            <div class="col-sm-4">
              <select class="form-control form-control-sm" v-model="selectedYear">
                <option v-for="year in years" :key="year" :value="year">{{ year }}</option>
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
             <button class="btn btn-primary btn-block">Submit</button>
             </div>
           </div>

        </form>

      </div>
    </div>
  </div>
</template>

<script>
export default {
  data (){
    return {
      years: [2017, 2018],
      selectedYear: 2017,
      payPeriods: ['Monthly','Annualy'],
      selectedPayPeriod: 'Monthly',
      salary: 0,
      age: 18,
      dependents: 0
    }
  }
}
</script>

<style>

</style>
