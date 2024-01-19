<template>
  <div class="container">
    <form @submit.prevent="startTest" class="mt-5">
      <div class="mb-3">
        <label class="form-label" for="apiUrlInput">API URL</label>
        <input v-model="apiUrl" type="text" class="form-control" id="apiUrlInput" placeholder="Api Adresini Giriniz:">
      </div>

      <br>
      <div class="mb-3">
        <label class="form-label" for="numRequestsInput">Request Sayısını Giriniz</label>
        <input v-model.number="numRequests" type="number" class="form-control" id="numRequestsInput"
          placeholder="Request Sayısını Giriniz:">
      </div>

      <br>
      <button type="submit" class="btn btn-primary" :disabled="isLoading">
        {{ isLoading ? 'Yükleniyor...' : 'Testi Başlat' }}
      </button>
    </form>

    <div v-if="isLoading" style="margin-top: 20px; text-align: center;">
      <p>Yükleniyor...</p>
    </div>

    <table v-if="responses.length" class="table" id="responseDataTable">
      <thead>
        <tr>
          <th scope="col">Request Sayısı</th>
          <th scope="col">Response Data</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(response, index) in responses" :key="index">
          <td>{{ index + 1 }}</td>
          <td>{{ response }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      apiUrl: '',
      numRequests: 0,
      isLoading: false,
      responses: []
    };
  },
  methods: {
    async startTest() {
      const loadingIndicator = document.getElementById("loadingIndicator");
      const authToken =
        "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJ5dWNlbC5ndW11c0BzYWJhaC5jb20udHIiLCJ1c2VySWQiOiI3IiwianRpIjoiZTAwNGQxMTAtZTAzYi00ZjQ1LWJkZDYtNzhhYzU2ZWIxYmY0IiwiaHR0cDovL3NjaGVtYXMubWljcm9zb2Z0LmNvbS93cy8yMDA4LzA2L2lkZW50aXR5L2NsYWltcy9yb2xlIjoiQWRtaW4iLCJleHAiOjE3MDU3MzExOTcsImlzcyI6Imh0dHA6Ly9sb2NhbGhvc3Q6NTAwMCIsImF1ZCI6Imh0dHA6Ly9sb2NhbGhvc3Q6NTAwMCJ9.Sk6sbqhu2TVZpjoMXcAEG4g0l41Jt2NOTipctJhnOyo";


      if (this.apiUrl && this.numRequests) {
        this.showLoadingIndicator(loadingIndicator);
        try {
          await this.sendMultipleRequests(this.numRequests, this.apiUrl, authToken);
        } finally {
          this.hideLoadingIndicator(loadingIndicator);
        }
      } else {
        alert("Lütfen Api Url Bilgisini ve request sayısını giriniz");
      }
    },
    async sendMultipleRequests(numRequests, apiUrl, authToken) {
      const requests = [];
      for (let i = 0; i < numRequests; i++) {
        requests.push(
          fetch(apiUrl, {
            headers: {
              Authorization: `Bearer ${authToken}`,
            },
          }).then((response) => response.json())
        );
      }

      try {
        const responses = await Promise.all(requests);
        responses.forEach((data,) => {
          this.displayResponseData(data);
        });
      } catch (error) {
        console.error("Request Sırasında bir hata oluştu", error.message);
      }
    },
    displayResponseData(responseData) {
      this.responses.push(JSON.stringify(responseData));
    },
    showLoadingIndicator() {
      this.isLoading = true;
    },
    hideLoadingIndicator() {
      this.isLoading = false;
    }
  }
};
</script>

<style scoped></style>
