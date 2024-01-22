<template>
  <q-page class="q-pa-md">
    <div class="q-pa-md row justify-center">
      <q-card
        style="width: 60vw;"
        flat
        bordered
      >

        <q-tabs
          v-model="mode"
          dense
          class="text-grey-7"
          active-color="indigo-10"
          indicator-color="deep-orange-8"
          align="justify"
        >
          <q-tab name="case" label="Case" />
          <q-tab name="batch" label="Batch" />
        </q-tabs>

        <q-tab-panels v-model="mode" animated>
          <q-tab-panel name="case" class="row justify-center">
            <CaseForm />
          </q-tab-panel>

          <q-tab-panel name="batch" class="row justify-center">
            <div class="col-md-8">
              <BatchForm />
            </div>
          </q-tab-panel>
        </q-tab-panels>
      </q-card>
    </div>
  </q-page>
</template>

<script>
import { defineComponent, reactive, ref } from 'vue'
import CaseForm from "src/components/CaseForm.vue"
import BatchForm from 'src/components/BatchForm.vue'

export default defineComponent({
  name: 'IndexPage',
  components: {
    CaseForm,
    BatchForm
  },
  setup() {
    return {
      mode: ref('case')
    }
  },
})
</script>
