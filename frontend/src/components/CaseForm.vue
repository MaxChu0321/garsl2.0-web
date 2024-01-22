<template>
  <q-form
      @submit="submitForm"
      class="q-gutter-md custom-bg"
    >
      <div class="row q-gutter-md justify-center">
        <q-input outlined color="indigo-9" type="number" step="0.01" v-model="form.tumor_size" label="Tumor size" />
        <!-- <q-input outlined color="indigo-9" type="number" step="0.01" v-model="form.lnast" label="lnAST" /> -->
        <q-input outlined color="indigo-9" type="number" step="0.01" v-model="form.tumor_number" label="Tumor number" />
        <q-input outlined color="indigo-9" type="number" step="0.01" v-model="form.height" label="Height" />
      </div>
      <div class="row q-gutter-md justify-center">
        <!-- <q-input outlined color="indigo-9" type="number" step="0.01" v-model="form.steatosis_grade" label="Steatosis grade" /> -->
        <q-input outlined color="indigo-9" type="number" step="0.01" v-model="form.k" label="K" />
        <q-input outlined color="indigo-9" type="number" step="0.01" v-model="form.ast" label="AST" />
        <q-input outlined color="indigo-9" type="number" step="0.01" v-model="form.hbsag" label="HBsAg" />
      </div>
      <div class="row q-gutter-md justify-center">
        <!-- <q-input outlined color="indigo-9" type="number" step="0.01" v-model="form.mvi" label="MVI" /> -->
        <q-input outlined color="indigo-9" type="number" step="0.01" v-model="form.histologic_grade" label="Histologic grade" />
        <q-input outlined color="indigo-9" type="number" step="0.01" v-model="form.afp" label="AFP" />
        <q-input outlined color="indigo-9" type="number" step="0.01" v-model="form.ishak" label="Ishak fibrosis stage" />
        <!-- <q-input outlined color="indigo-9" type="number" step="0.01" v-model="form.steatosis_grade2" label="Steatosis grade2" /> -->
      </div>
      <div class="row q-gutter-md justify-center">
            <div class="q-pa-sm rounded-borders" :class="$q.dark.isActive ? 'bg-grey-9' : 'bg-grey-2'">
            Microvascular invasion
            <q-option-group
                :options="[
                { label: '0', value: 0, color: 'green' },
                { label: '1', value: 1, color: 'red' }
                ]"
                type="radio"
                inline
                v-model="form.mvi"
            />
            </div>
            <div class="q-pa-sm rounded-borders" :class="$q.dark.isActive ? 'bg-grey-9' : 'bg-grey-2'">
            ALBIgrade
            <q-option-group
                :options="[
                { label: '0', value: 0, color: 'green' },
                { label: '1', value: 1, color: 'yellow' },
                { label: '2', value: 2, color: 'red' }
                ]"
                type="radio"
                inline
                v-model="form.albigrade"
            />
            </div>
        
            <div class="q-pa-sm rounded-borders" :class="$q.dark.isActive ? 'bg-grey-9' : 'bg-grey-2'">
            BCLC stage
            <q-option-group
                :options="[
                { label: '0', value: 0, color: 'green' },
                { label: '1', value: 1, color: 'yellow' },
                { label: '2', value: 2, color: 'orange' },
                { label: '3', value: 3, color: 'red' },
                ]"
                type="radio"
                inline
                v-model="form.bclc"
            />
            </div>
            <div class="q-pa-sm rounded-borders" :class="$q.dark.isActive ? 'bg-grey-9' : 'bg-grey-2'">
            Steatosis grade
            <q-option-group
                :options="[
                { label: '0', value: 0, color: 'green' },
                { label: '1', value: 1, color: 'yellow' },
                { label: '2', value: 2, color: 'orange' },
                { label: '3', value: 3, color: 'red' },
                ]"
                type="radio"
                inline
                v-model="form.steatosis_grade"
            />
            </div>
            <!-- <div class="q-pa-sm rounded-borders" :class="$q.dark.isActive ? 'bg-grey-9' : 'bg-grey-2'">
            class_Histologic grade
            <q-option-group
                :options="[
                { label: '0', value: 0, color: 'green' },
                { label: '1', value: 1, color: 'yellow' },
                { label: '2', value: 2, color: 'orange' },
                { label: '3', value: 3, color: 'red' },
                ]"
                type="radio"
                inline
                v-model="form.class_histologic_grade"
            />
            </div>
            <div class="q-pa-sm rounded-borders" :class="$q.dark.isActive ? 'bg-grey-9' : 'bg-grey-2'">
            class_AFP
            <q-option-group
                :options="[
                { label: '0', value: 0, color: 'green' },
                { label: '1', value: 1, color: 'yellow' },
                { label: '2', value: 2, color: 'orange' },
                { label: '3', value: 3, color: 'red' },
                ]"
                type="radio"
                inline
                v-model="form.class_afp"
            />
            </div>
            <div class="q-pa-sm rounded-borders" :class="$q.dark.isActive ? 'bg-grey-9' : 'bg-grey-2'">
            class_Steatosis grade
            <q-option-group
                :options="[
                { label: '0', value: 0, color: 'green' },
                { label: '1', value: 1, color: 'red' },
                ]"
                type="radio"
                inline
                v-model="form.class_steatosis_grade"
            />
            </div> -->
        </div>
        <div class="row q-gutter-md justify-end">
            <q-btn unelevated color="teal-6" type="button" label="Demo" @click="demo" />
            <q-btn unelevated color="teal-6" type="submit" label="Submit" />
        </div>
    </q-form>
</template>

<script>
import { defineComponent, reactive, ref } from 'vue'
import { api } from "boot/axios"
import { useRouter } from 'vue-router'
import { useJobStore } from 'stores/job'
import { useTestCase } from '../helpers/useDemo'

export default defineComponent({
  name: 'CaseForm',
  setup () {
    const $router = useRouter()
    const job_store = useJobStore()

    let { demo_case } = useTestCase()
    // console.log(demo_case)

    const form = reactive({
      'tumor_size': null,
      // 'lnast': null,
      'tumor_number': null,
      'ishak': null,
      'height': null,
      'steatosis_grade': null,
      'k': null,
      'ast': null,
      'hbsag': null,
      'mvi': null,
      'albigrade': null,
      'bclc': null,
      'histologic_grade': null,
      'afp': null,
      // 'steatosis_grade2': null,
      
    })

    const demo = () => {
      form.tumor_size= demo_case.tumor_size,
      // form.lnast= demo_case.lnast,
      form.tumor_number= demo_case.tumor_number,
      form.ishak= demo_case.ishak,
      form.height= demo_case.height,
      form.steatosis_grade= demo_case.steatosis_grade,
      form.k= demo_case.k,
      form.ast= demo_case.ast,
      form.hbsag= demo_case.hbsag,
      form.mvi= demo_case.mvi,
      form.albigrade= demo_case.albigrade,
      form.bclc= demo_case.bclc,
      form.histologic_grade= demo_case.histologic_grade,
      form.afp= demo_case.afp
      // form.steatosis_grade2= demo_case.steatosis_grade2
    }

    const submitForm = () =>{
      api.post("submit/case", form)
        .then((response)=>{
          job_store.updateJobResult(response.data[0])
          job_store.updateOriginalFormValues(form)
          console.log(job_store.risk_score)
          $router.push('/result/case')
        })
        .catch((error)=>{
          console.log(error.message)
        })
    }

    return {form, submitForm, demo}
  }
})
</script>
<style>
.custom-bg {
  background-color: rgba(255, 255, 255, 0.7); /* 白色背景，50% 透明度 */
}
</style>