<template>
  <div class="home">
    <div
      class="mb-4"
      style="height: 56px"
    ></div>
    <div class="container-fluid">
      <div class="row">
        <div class="col-lg-6">
          <h4 class="text-center">Predict Input</h4>
          <hr />
          <div class="form-group">
            <label for="medical-record">
              Medical record
            </label>
            <textarea
              class="form-control shadow-sm"
              id="medical-record"
              rows="20"
              v-model="medical_record"
            ></textarea>
            <p class="text-center mt-3">
              <a
                href="javascript:;"
                class="btn btn-success shadow-sm"
                v-bind:class="{ disabled: disabled }"
                v-on:click="predict"
              >
                <span>Predict</span><span v-if="disabled">ing...</span>&nbsp;<span v-if="!disabled">Now</span>
                <div
                  class="spinner-border text-light spinner-border-sm"
                  role="status"
                  v-if="disabled"
                >
                  <span class="sr-only">Loading...</span>
                </div>
              </a>
            </p>
          </div>
        </div>
        <div class="col-lg-6">
          <h4 class="text-center">Result</h4>
          <hr />
          <div
            class="alert alert-light text-center shadow-sm"
            role="alert"
            v-if="!result"
          >
            No predict request, please input the medical record then press
            <kbd>Predict Now</kbd> bottom.
          </div>
          <div v-else-if="result">
            <table class="table">
              <thead>
                <tr>
                  <th scope="col">#</th>
                  <th scope="col">ICD-10 Code</th>
                  <th scope="col">Title</th>
                  <th scope="col">Probability</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="(item, index) in result"
                  :key="index"
                  :style="background(item.probability, item.mark)"
                >
                  <th scope="row">{{ index + 1 }}</th>
                  <td>{{ item.code }}</td>
                  <td>{{ item.title }}</td>
                  <td>{{ (item.probability*100).toFixed(3) }}%</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div
            class="text-center"
            v-else-if="predicting"
          >
            <div
              class="spinner-border"
              role="status"
            >
              <span class="sr-only">Loading...</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "Home",
    data() {
      return {
        disabled: false,
        predicting: false,
        medical_record: "",
        result: null
      };
    },
    methods: {
      predict: function() {
        this.disabled = true;
        this.predicting = true;
        this.$http
          .post("http://localhost:8888/api/predict", {
            pre_data: this.medical_record
          })
          .then(response => {
            this.result = response.data.data;
            this.predicting = false;
            this.disabled = false;
          })
          .catch(err => {
            console.log(err);
          });
      },
      background(predicting, mark) {
        return (
          "background: rgba(" +
          (mark ? "195, 230, 203, " : "245, 198, 203, ") +
          predicting +
          ")"
        );
      }
    }
  };
</script>
