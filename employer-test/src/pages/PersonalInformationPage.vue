<script setup>
import { ref, onMounted, TransitionGroup, defineAsyncComponent } from 'vue'
import UserImage from '../assets/public/UserImage.png'

let formActive = ref(false)
let employee = ref()

let backupEmployee = undefined

const fields = ref([
  {
    label: 'Join Date',
    slug: 'joinDate',
    type: 'date',
    value: ''
  },
  {
    label: 'Initial Join Date',
    slug: 'initialJoinDate',
    type: 'date',
    value: ''
  },
  {
    label: 'Terminate Date',
    slug: 'terminateDate',
    type: 'date',
    value: ''
  },
  {
    label: 'Type of Contract',
    slug: 'typeOfContract',
    type: 'text',
    value: ''
  },
  {
    label: 'Work experience',
    slug: 'workExperience',
    type: 'select',
    option: [
      {
        value: '1',
        labelSelect: '1 tahun'
      },
      {
        value: '2',
        labelSelect: '2 tahun'
      },
      {
        value: '3',
        labelSelect: '3 tahun'
      }
    ],
    selectedOption: ''
  },
  {
    label: 'Recruitment Type',
    slug: 'recuitmentType',
    type: 'select',
    option: [
      {
        value: 'job-portal',
        labelSelect: 'Job Portal'
      },
      {
        value: 'recruit',
        labelSelect: 'Recruit'
      }
    ],
    selectedOption: ''
  },
  {
    label: 'Service Length Total',
    slug: 'serviceLengthTotal',
    type: 'text',
    value: ''
  },
  {
    label: 'Employee status',
    slug: 'employeeStatus',
    type: 'select',
    option: [
      {
        value: 'active',
        labelSelect: 'active'
      },
      {
        value: 'inactive',
        labelSelect: 'inactive'
      }
    ],
    selectedOption: ''
  },
  {
    label: 'Exit Reason',
    slug: 'exitReason',
    type: 'select',
    option: [
      {
        value: 'sick',
        labelSelect: 'Sick'
      },
      {
        value: 'drop-out',
        labelSelect: 'Drop Out'
      }
    ],
    selectedOption: ''
  },
  {
    label: 'Exit Date',
    slug: 'exitDate',
    type: 'text',
    value: ''
  },
  {
    label: 'Resigner Tender Date',
    slug: 'resignerTenderDate',
    type: 'text',
    value: ''
  }
])

function showForm() {
  console.log('klik')
  formActive.value = !formActive.value
}

async function fetchData() {
  const response = await fetch('https://66acda18f009b9d5c7339e2f.mockapi.io/employee')
  const data = await response.json()

  console.log(data)
  employee.value = data[0]
  fields.value.forEach((item) => {
    if (item.type === 'select') {
      item.selectedOption = data[0][item.slug]
    } else if (item.type === 'date') {
      item.value = new Date(data[0][item.slug]).toISOString().split('T')[0]
    } else {
      item.value = data[0][item.slug]
    }
  })
}

function backupData() {
  backupEmployee = employee.value
}

async function discardForms() {
  console.log('ini backup', backupEmployee)
  fields.value.forEach((item) => {
    if (item.type === 'select') {
      item.selectedOption = backupEmployee[item.slug]
    } else if (item.type === 'date') {
      item.value = new Date(backupEmployee[item.slug]).toISOString().split('T')[0]
    } else {
      item.value = backupEmployee[item.slug]
    }
  })
  const response = await fetch(
    `https://66acda18f009b9d5c7339e2f.mockapi.io/employee/${employee.value.id}`,
    {
      method: 'PUT',
      body: JSON.stringify(backupEmployee),
      headers: { 'content-type': 'application/json' }
    }
  )
  console.log(await response.json())
  fetchData()
}

async function saveForms() {
  const data = {}
  fields.value.forEach((item) => {
    if (item.type === 'select') {
      data[item.slug] = item.selectedOption
    } else {
      data[item.slug] = item.value
    }
  })
  console.log('setelah diubah', data)
  const response = await fetch(
    `https://66acda18f009b9d5c7339e2f.mockapi.io/employee/${employee.value.id}`,
    {
      method: 'PUT',
      body: JSON.stringify(data),
      headers: { 'content-type': 'application/json' }
    }
  )
  console.log(await response.json())
  fetchData()
}

onMounted(async () => {
  await fetchData()
  backupData()
})
</script>

<template>
  <div class="flex flex-col gap-6">
    <div class="flex flex-row p-8 bg-white items-center justify-between rounded-2xl">
      <div class="flex gap-8 items-center flex-1">
        <img :src="UserImage" alt="" class="w-24 h-24 object-cover rounded-full" />
        <div class="flex flex-col gap-3">
          <p class="font-bold text-xl">{{ employee?.name }}</p>
          <p class="font-bold">{{ employee?.employeeNumber }}</p>
        </div>
      </div>

      <div class="flex flex-col w-[40%] gap-4">
        <p>Departement: <span class="font-bold">Sales &amp; Marketing</span></p>
        <p>Position: <span class="font-bold">Marketing Manager</span></p>
      </div>
    </div>

    <div class="flex bg-white py-8 px-10 items-center justify-between rounded-2xl">
      <div class="flex w-full justify-between">
        <p class="font-bold text-[22px]">Personal Info</p>
        <svg
          width="36"
          height="36"
          viewBox="0 0 36 36"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            fill-rule="evenodd"
            clip-rule="evenodd"
            d="M11.3063 14.5879C11.719 14.1234 12.43 14.0814 12.8946 14.494L18.1474 19.1602L23.4003 14.494C23.8648 14.0814 24.5759 14.1234 24.9885 14.5879C25.4011 15.0524 25.3591 15.7635 24.8946 16.1761L18.8946 21.506C18.4684 21.8846 17.8264 21.8846 17.4003 21.506L11.4003 16.1761C10.9358 15.7635 10.8937 15.0524 11.3063 14.5879Z"
            fill="black"
          />
        </svg>
      </div>
    </div>

    <div class="flex flex-col bg-white py-8 px-10 items-center justify-between rounded-2xl gap-8">
      <div class="flex w-full justify-between">
        <p class="font-bold text-[22px]">Employment Info</p>
        <button @click="showForm()">
          <svg
            width="36"
            height="36"
            viewBox="0 0 36 36"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
            :class="[formActive ? '' : 'hidden']"
          >
            <path
              fill-rule="evenodd"
              clip-rule="evenodd"
              d="M11.3058 21.4121C11.7185 21.8766 12.4295 21.9186 12.8941 21.506L18.1469 16.8398L23.3998 21.506C23.8643 21.9186 24.5754 21.8766 24.988 21.4121C25.4006 20.9476 25.3586 20.2365 24.8941 19.8239L18.8941 14.494C18.4679 14.1154 17.8259 14.1154 17.3998 14.494L11.3998 19.8239C10.9353 20.2365 10.8932 20.9476 11.3058 21.4121Z"
              fill="black"
            />
          </svg>
          <svg
            width="36"
            height="36"
            viewBox="0 0 36 36"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
            :class="[formActive ? 'hidden' : '']"
          >
            <path
              fill-rule="evenodd"
              clip-rule="evenodd"
              d="M11.3063 14.5879C11.719 14.1234 12.43 14.0814 12.8946 14.494L18.1474 19.1602L23.4003 14.494C23.8648 14.0814 24.5759 14.1234 24.9885 14.5879C25.4011 15.0524 25.3591 15.7635 24.8946 16.1761L18.8946 21.506C18.4684 21.8846 17.8264 21.8846 17.4003 21.506L11.4003 16.1761C10.9358 15.7635 10.8937 15.0524 11.3063 14.5879Z"
              fill="black"
            />
          </svg>
        </button>
      </div>
      <div class="grid grid-cols-2 gap-4 w-full" :class="{ hidden: !formActive }">
        <div v-for="(field, index) in fields" :key="index">
          <div
            v-if="field.type === 'date'"
            :class="[
              'flex flex-row w-full p-2 border-b-black border-b',
              index === fields.length - 1 ? 'border-b-0' : ''
            ]"
          >
            <p class="w-1/2 font-bold">{{ field.label }}</p>
            <input
              class="text-black/70 flex-1 w-full capitalize"
              type="date"
              v-model="field.value"
            />
          </div>
          <div
            v-else-if="field.type === 'select'"
            :class="[
              'flex flex-row w-full p-2 border-b-black border-b',
              index === fields.length - 1 ? 'border-b-0' : ''
            ]"
          >
            <label for="selected" class="w-1/2 font-bold">{{ field.label }}</label>

            <select
              class="flex-1 capitalize text-black/70"
              name="selected"
              id="selected"
              v-model="field.selectedOption"
            >
              <option v-for="(option, index) in field.option" :key="index" :value="option.value">
                {{ option.labelSelect }}
              </option>
            </select>
          </div>
          <div
            v-else
            :class="[
              'flex flex-row w-full p-2 border-b-black border-b',
              index === fields.length - 1 ? 'border-b-0' : ''
            ]"
          >
            <p class="w-1/2 font-bold">{{ field.label }}</p>
            <input
              class="text-black/70 flex-1 w-full capitalize"
              type="text"
              v-model="field.value"
            />
          </div>
        </div>
      </div>
    </div>

    <div class="flex bg-white py-8 px-10 items-center justify-between rounded-2xl">
      <div class="flex w-full justify-between">
        <p class="font-bold text-[22px]">Assigntment</p>
        <svg
          width="36"
          height="36"
          viewBox="0 0 36 36"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            fill-rule="evenodd"
            clip-rule="evenodd"
            d="M11.3063 14.5879C11.719 14.1234 12.43 14.0814 12.8946 14.494L18.1474 19.1602L23.4003 14.494C23.8648 14.0814 24.5759 14.1234 24.9885 14.5879C25.4011 15.0524 25.3591 15.7635 24.8946 16.1761L18.8946 21.506C18.4684 21.8846 17.8264 21.8846 17.4003 21.506L11.4003 16.1761C10.9358 15.7635 10.8937 15.0524 11.3063 14.5879Z"
            fill="black"
          />
        </svg>
      </div>
    </div>
  </div>

  <div
    :class="[
      'w-full fixed bg-white px-14 flex flex-row items-center py-[14px] gap-2 justify-between bottom-0 left-0',
      formActive ? '' : 'hidden'
    ]"
  >
    <p>Work Experience has changed</p>
    <div class="flex flex-row gap-2">
      <button class="px-6 py-3 rounded-xl bg-[#DFE5F0] font-bold" @click="discardForms()">
        Discard
      </button>
      <button class="px-6 py-3 rounded-xl bg-[#58C5C8] font-bold text-white" @click="saveForms()">
        Save Changes
      </button>
    </div>
  </div>
</template>
