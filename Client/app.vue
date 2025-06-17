<template>
  <v-app>
    <!-- Toolbar at the top of the screen -->
    <v-app-bar color="primary" height="48">
      <!-- Title of the application with adjusted margin and font size -->
      <v-app-bar-title class="text-h6 ms-3">
        <v-btn @click="drawer = !drawer">
          <Icon name="uil:bars" size="24" />
        </v-btn>

        <span class="ms-1">What is to do?</span>
      </v-app-bar-title>

      <!-- Spacer to push the following elements to the right -->
      <Icon name="uil:calender" size="24" />
      <span class="ms-2 text-subtitle-2">{{ currentDate }}</span>
      <v-btn @click="changeLocal" icon>
        <Icon name="uil:clock" size="24"/>
      </v-btn>
      <span class="ms-2 text-subtitle-2">{{ currentTime }}</span>
      <v-spacer />

      <!-- Menu icons on the right side of the toolbar -->
      <v-btn v-for="menu in menus" :key="menu.name" @click="navigateTo(menu.path)">
        <Icon :name="menu.icon" size="24" />
      </v-btn>
    </v-app-bar>

    <!-- Navigation drawer for menu actions -->
    <v-navigation-drawer v-model="drawer">
      <v-list>
        <!-- Menu items in the drawer with icons -->
        <v-list-item v-for="menu in menus" :key="menu.name" @click="navigateTo(menu.path)">
          <v-list-item-title>
            {{ menu.name }}
          </v-list-item-title>
        </v-list-item>

      </v-list>
    </v-navigation-drawer>

    <!-- Main content of the application -->
    <v-main>
      <NuxtPage />
    </v-main>

    <!-- Smaller footer at the bottom of the screen -->
    <v-footer app color="secondary" height="30">
      <v-container class="text-caption text-center">
        © {{ new Date().getFullYear() }} Suraball - All Rights Reserved
      </v-container>
    </v-footer>
  </v-app>
</template>

<script setup>
const drawer = shallowRef(false)
const currentDate = ref('')
const currentTime = ref('')
let hour12 = ref(false)

// Get Timezone
const locale = Intl.DateTimeFormat().resolvedOptions().locale

// Function to update time
const updateTime = () => {
  const now = new Date()
  currentDate.value = now.toLocaleDateString(locale, {
    day: 'numeric',
    month: 'long',
    year: 'numeric'
  })
  currentTime.value = now.toLocaleTimeString(locale, {
    hour: '2-digit',
    minute: '2-digit',
    second: '2-digit',
    hour12
  })
}

// Update time every second
onMounted(() => {
  updateTime()
  setInterval(updateTime, 1000)
})

const changeLocal = () => {
  hour12 = !hour12
}

const menus = [
  {
    name: 'Home',
    icon: 'uil:home',
    path: '/'
  },
  {
    name: 'Task',
    icon: 'uil:comment-info',
    path: '/task'
  },
  {
    name: 'Profile',
    icon: 'uil:user',
    path: '/profile'
  }
]
</script>
