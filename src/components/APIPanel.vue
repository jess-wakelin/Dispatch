<template>
  <div class="p-4">
    <form data-form="">
      <div class="request-type">
        <select>
          <option value="GET">GET</option>
          <option value="POST">POST</option>
          <option value="PUT">PUT</option>
          <option value="PATCH">PATCH</option>
          <option value="DELETE">DELETE</option>
        </select>
        <input required type="url" placeholder="https://example.com" />
        <button type="submit">Send</button>
      </div>

      <TabSystem :dynamic="false" :injected-tabs="tabs" class="request-body">
        <template
          #tab-1
          class="tab-pane fade show active"
          id="query-params"
          role="tabpanel"
          aria-labelledby="query-params-tab"
        >
          <div data-query-params="" v-for="(param, i) in queryParams" :key="i">
            <input
              type="text"
              name="key"
              v-model="param.key"
              @change="updateJSON"
            />
            <input
              type="text"
              name="value"
              v-model="param.value"
              @change="updateJSON"
            />
            <button type="button" name="remove" @click="removeQueryParam(i)">
              Remove
            </button>
          </div>
          <button
            data-add-query-param-btn=""
            class="mt-2 btn btn-outline-success"
            type="button"
            @click="addQueryParam"
          >
            Add
          </button>
        </template>

        <template
          #tab-2
          class="tab-pane fade"
          id="request-headers"
          role="tabpanel"
          aria-labelledby="request-headers-tab"
        >
          <div data-request-headers="" v-for="(header, i) in headers" :key="i">
            <input
              type="text"
              name="key"
              v-model="header.key"
              @change="updateJSON"
            />
            <input
              type="text"
              name="value"
              v-model="header.value"
              @change="updateJSON"
            />
            <button type="button" name="remove" @click="removeHeader(i)">
              Remove
            </button>
          </div>
          <button
            data-add-request-header-btn=""
            class="mt-2 btn btn-outline-success"
            type="button"
            @click="addHeader"
          >
            Add
          </button>
        </template>

        <template
          #tab-3
          class="tab-pane fade"
          id="json"
          role="tabpanel"
          aria-labelledby="json-tab"
        >
          <div
            data-json-request-body=""
            class="overflow-auto"
            style="max-height: 200px"
          ></div>
        </template>
      </TabSystem>
    </form>

    <div class="mt-5 d-none response" data-response-section="">
      <h3>Response</h3>
      <div class="d-flex my-2">
        <div class="me-3">Status: <span data-status=""></span></div>
        <div class="me-3">Time: <span data-time=""></span>ms</div>
        <div class="me-3">Size: <span data-size=""></span></div>
      </div>

      <ul class="nav nav-tabs" role="tablist">
        <li class="nav-item" role="presentation">
          <button
            class="nav-link active"
            id="body-tab"
            data-bs-toggle="tab"
            data-bs-target="#body"
            type="button"
            role="tab"
            aria-controls="body"
            aria-selected="true"
          >
            Body
          </button>
        </li>
        <li class="nav-item" role="presentation">
          <button
            class="nav-link"
            id="response-headers-tab"
            data-bs-toggle="tab"
            data-bs-target="#response-headers"
            type="button"
            role="tab"
            aria-controls="response-headers"
            aria-selected="false"
          >
            Headers
          </button>
        </li>
      </ul>

      <div class="tab-content p-3 border-top-0 border">
        <div
          class="tab-pane fade show active"
          id="body"
          role="tabpanel"
          aria-labelledby="body-tab"
        >
          <div
            data-json-response-body
            class="overflow-auto"
            style="max-height: 200px"
          ></div>
        </div>
        <div
          class="tab-pane fade"
          id="response-headers"
          role="tabpanel"
          aria-labelledby="response-headers-tab"
        >
          <div
            style="
              display: grid;
              grid-template-columns: auto 1fr;
              gap: 0.5rem 2rem;
            "
            data-response-headers
          ></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TabSystem from "@/components/TabSystem.vue";

export default {
  name: "APIPanel",
  components: {
    TabSystem,
  },
  data() {
    return {
      tabs: [
        {
          title: "Query Params",
        },
        {
          title: "Headers",
        },
        {
          title: "JSON",
        },
      ],
      queryParams: [
        {
          key: "",
          value: "",
        },
      ],
      headers: [
        {
          key: "",
          value: "",
        },
      ],
    };
  },
  methods: {
    addQueryParam() {
      this.queryParams.push({
        key: "",
        value: "",
      });
      this.updateJSON();
    },
    removeQueryParam(id) {
      this.queryParams.splice(id, 1);
      this.updateJSON();
    },
    addHeader() {
      this.headers.push({
        key: "",
        value: "",
      });
      this.updateJSON();
    },
    removeHeader(id) {
      this.headers.splice(id, 1);
      this.updateJSON();
    },
    updateJSON() {
      console.log(
        JSON.stringify({
          params: this.queryParams.filter((item) => item.key !== ""),
          headers: this.headers.filter((item) => item.key !== ""),
        })
      );
      // style key-value pairs
      //successfully update JSON
      // style response section
      // implement request making and response receiving
    },
  },
};
</script>

<style lang="sass" scoped>
.request-type
  display: flex
  height: 2rem
  padding: 1rem 0

  &>*
    border: 1px solid #dedede

  :first-child
    border-radius: 6px 0 0 6px
    border-right: none

  :last-child
    border-radius: 0 6px 6px 0

  input
    flex: 1 0 auto

  button
    outline: none
    border: none
    background-color: lightblue
    flex: 0 0 5rem

.request-body
  padding: 1rem 0

.response
  padding: 1rem 0
</style>