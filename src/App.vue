<template>
  <div id="app">
    <div class="container">
      <div class="row">
        <div class="col-12 col-md-8 col-lg-8 mx-auto">
          <vue-query-builder
            :maxDepth="4"
            :labels="labels"
            :rules="rules"
            v-model="query"
          >
          </vue-query-builder>
        </div>
      </div>
      <p>Generated output:</p>
      <pre>{{ JSON.stringify(this.query, null, 2) }}</pre>
    </div>
  </div>
</template>

<script>
import VueQueryBuilder from "vue-query-builder";
import VueSlideBar from "vue-slide-bar";

export default {
  name: "App",
  components: {
    VueQueryBuilder,
  },

  data() {
    return {
      rules: [
        {
          type: "text",
          id: "make",
          label: "Make",
        },
        {
          type: "text",
          id: "model",
          label: "Model",
        },
        {
          type: "numeric",
          id: "year",
          label: "Year",
        },
        {
          type: "radio",
          id: "transmission",
          label: "Transmission",
          choices: [
            { label: "Auto", value: "auto" },
            { label: "Manual", value: "manual" },
          ],
        },
        {
          type: "select",
          id: "isfinance",
          label: "is Finance",
          choices: [
            { label: "Yes", value: "Yes" },
            { label: "NO", value: "NO" },
          ],
        },
        {
          type: "custom-component",
          id: "percentage",
          label: "Percentage",
          operators: ["="],
          component: VueSlideBar,
          default: 50,
        },
      ],
      labels: {
        matchType: "Match Type",
        matchTypes: [
          { id: "and", label: "AND" },
          { id: "or", label: "OR" },
        ],
        addRule: "Add Rule",
        removeRule: "&times;",
        addGroup: "Add Group",
        removeGroup: "&times;",
        textInputPlaceholder: "value",
      },
      query: {
        logicalOperator: "and",
        children: [
          {
            type: "query-builder-group",
            query: {
              logicalOperator: "or",
              children: [
                {
                  type: "query-builder-rule",
                  query: {
                    rule: "make",
                    value: "Toyota",
                    operator: "contains",
                  },
                },
                {
                  type: "query-builder-rule",
                  query: {
                    rule: "model",
                    value: "Camry",
                    operator: "equals",
                  },
                },
                {
                  type: "query-builder-group",
                  query: {
                    logicalOperator: "and",
                    children: [
                      {
                        type: "query-builder-rule",
                        query: {
                          rule: "year",
                          operator: ">=",
                          operand: "Year",
                          value: "2000",
                        },
                      },
                      {
                        type: "query-builder-rule",
                        query: {
                          rule: "year",
                          operator: "<=",
                          operand: "Year",
                          value: "2020",
                        },
                      },
                    ],
                  },
                },
                {
                  type: "query-builder-rule",
                  query: {
                    rule: "transmission",
                    operand: "Transmission",
                    value: "manual",
                  },
                },
                {
                  type: "query-builder-rule",
                  query: {
                    rule: "isfinance",
                    operand: "is Finance",
                    value: "Yes",
                  },
                },
                {
                  type: "query-builder-rule",
                  query: {
                    rule: "percentage",
                    operator: "=",
                    operand: "Percentage",
                    value: 50,
                  },
                },
              ],
            },
          },
        ],
      },
    };
  },
};
</script>
<style>
.vqb-custom-component-wrap {
  flex-grow: 1;
  padding-right: 15px;
}
pre {
  display: block;
  font-size: 87.5%;
  color: #212529;
  border: solid 2px green;
  border-radius: 10px;
  padding: 10px;
}
</style>