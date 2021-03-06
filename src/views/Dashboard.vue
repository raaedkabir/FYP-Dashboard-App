<template>
  <div class="container">
    <div class="sidebar">
      <h2>FYP Project</h2>
      <hr />
      <router-link class="list-item" to="/dashboard">
        <mdicon name="view-dashboard" />
        <span>Home</span>
      </router-link>
      <router-link class="list-item" to="/dashboard/user">
        <mdicon name="account" />
        <span>User Profile</span>
      </router-link>
      <router-link v-if="isAdmin" class="list-item" to="/dashboard/employees">
        <mdicon name="account-group" />
        <span>Employees</span>
      </router-link>
      <router-link class="list-item" to="/dashboard/notifications">
        <mdicon name="bell" />
        <span>Notifications</span>
      </router-link>
      <router-link v-if="isAdmin" class="list-item" to="/dashboard/maps">
        <mdicon name="map-marker" />
        <span>Maps</span>
      </router-link>
    </div>
    <div class="content">
      <header class="header">
        <div class="header--title">{{ routeName }}</div>
        <div class="header--actions">
          <router-link class="notification" to="/dashboard/notifications">
            <mdicon name="bell" />
            <span v-if="imageMissing" class="badge">1</span>
          </router-link>
          <mdicon name="logout" @click="logout" />
        </div>
      </header>
      <router-view />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, watchEffect } from 'vue';
import { useRoute, useRouter } from 'vue-router';

export default defineComponent({
  components: {},
  setup() {
    const route = useRoute();
    const router = useRouter();
    const isAdmin = JSON.parse(localStorage.getItem('isAdmin') as string);
    const imageMissing = !localStorage.getItem('imageLink');

    let routeName = ref(route.name);

    watchEffect(() => {
      routeName.value = route.name;
    });

    const logout = () => {
      localStorage.clear();
      router.replace('/');
    };

    return {
      routeName,
      logout,
      isAdmin,
      imageMissing,
    };
  },
});
</script>

<style lang="scss" scoped>
.container {
  display: flex;

  .sidebar {
    padding: 1em 2em;
    min-height: 100vh;
    color: #fff;
    background: rgba($color: #000000, $alpha: 0.8);

    hr {
      border: 1px solid #fff;
      margin: 0.5em -0.75em 1em;
    }

    .list-item {
      display: flex;
      align-items: center;
      margin: 10px 0 0;
      padding: 10px 35px 10px 15px;
      color: currentColor;
      text-decoration: none;
      border-radius: 3px;
      font-weight: 300;
      transition: all 0.2s ease;

      &.router-link-exact-active {
        box-shadow: 0 12px 20px -10px rgb(0 172 193 / 28%), 0 4px 20px 0 rgb(0 0 0 / 12%),
          0 7px 8px -5px rgb(0 172 193 / 20%);
        background: var(--clr-primary-400);
      }

      .mdi {
        margin-right: 1.5rem;
        transform: translateY(-1px);
      }
    }
  }

  .content {
    width: 100%;
    display: flex;
    flex-direction: column;

    .header {
      display: flex;
      justify-content: space-between;
      padding: 1.5rem 2rem;
      margin-bottom: 4rem;

      &--title {
        font-weight: 300;
        font-size: 2rem;
      }

      &--actions {
        .mdi {
          color: var(--clr-neutral-700);
          margin-left: 1.5rem;
          cursor: pointer;
        }

        .notification {
          position: relative;
        }

        .badge {
          position: absolute;
          line-height: 1.2;
          top: -10px;
          right: -10px;
          width: 20px;
          height: 20px;
          text-align: center;
          border-radius: 50%;
          background: red;
          color: white;
        }
      }
    }
  }
}
</style>
