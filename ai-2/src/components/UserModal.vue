<script setup lang="ts">
import { defineProps, defineEmits } from "vue";
import type { User } from "../types/User";

const props = defineProps<{
  user: User | null;
  isOpen: boolean;
}>();

const emit = defineEmits<{
  close: [];
}>();

const closeModal = () => {
  emit("close");
};

const handleBackdropClick = (event: MouseEvent) => {
  if ((event.target as HTMLElement).classList.contains("modal-backdrop")) {
    closeModal();
  }
};

const openMapLink = (lat: string, lng: string) => {
  window.open(`https://www.google.com/maps?q=${lat},${lng}`, "_blank");
};
</script>

<template>
  <Transition name="modal">
    <div v-if="isOpen && user" class="modal-backdrop" @click="handleBackdropClick">
      <div class="modal-content">
        <button @click="closeModal" class="close-btn" aria-label="Close modal">×</button>

        <div class="modal-header">
          <h2 class="user-name">{{ user.name }}</h2>
          <p class="user-email">{{ user.email }}</p>
        </div>

        <div class="modal-body">
          <section class="info-section">
            <h3>Address</h3>
            <p class="address-line">{{ user.address.street }}, Apt. {{ user.address.suite }}</p>
            <p class="address-line">{{ user.address.city }}, {{ user.address.zipcode }}</p>
            <button
              @click="openMapLink(user.address.geo.lat, user.address.geo.lng)"
              class="map-link"
            >
              📍 View on map
            </button>
          </section>

          <section class="info-section">
            <h3>Contact</h3>
            <div class="contact-item"><strong>Phone:</strong> {{ user.phone }}</div>
            <div class="contact-item">
              <strong>Website:</strong>
              <a :href="`https://${user.website}`" target="_blank" class="website-link">
                {{ user.website }}
              </a>
            </div>
          </section>

          <section class="info-section">
            <h3>Company</h3>
            <div class="company-item"><strong>Name:</strong> {{ user.company.name }}</div>
            <div class="company-item">
              <strong>Catchphrase:</strong> {{ user.company.catchPhrase }}
            </div>
            <div class="company-item"><strong>Business:</strong> {{ user.company.bs }}</div>
          </section>
        </div>
      </div>
    </div>
  </Transition>
</template>

<style lang="scss" scoped>
.modal-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 20px;
}

.modal-content {
  background: white;
  border-radius: 12px;
  width: 100%;
  max-width: 600px;
  max-height: 90vh;
  overflow-y: auto;
  position: relative;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
}

.close-btn {
  position: absolute;
  top: 16px;
  right: 16px;
  background: none;
  border: none;
  font-size: 32px;
  color: #6c757d;
  cursor: pointer;
  padding: 8px;
  border-radius: 4px;
  transition: all 0.2s ease;
  line-height: 1;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;

  &:hover {
    background-color: #f8f9fa;
    color: #212529;
  }

  &:focus {
    outline: none;
    box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.25);
  }
}

.modal-header {
  padding: 32px 32px 24px;
  border-bottom: 1px solid #e9ecef;

  .user-name {
    font-size: 28px;
    font-weight: 700;
    color: #212529;
    margin: 0 0 8px 0;
  }

  .user-email {
    font-size: 16px;
    color: #007bff;
    margin: 0;
  }
}

.modal-body {
  padding: 32px;
}

.info-section {
  margin-bottom: 32px;

  &:last-child {
    margin-bottom: 0;
  }

  h3 {
    font-size: 20px;
    font-weight: 600;
    color: #212529;
    margin: 0 0 16px 0;
  }

  .address-line {
    font-size: 16px;
    color: #495057;
    margin: 0 0 8px 0;
  }

  .map-link {
    background: none;
    border: none;
    color: #007bff;
    font-size: 16px;
    cursor: pointer;
    padding: 8px 0;
    transition: color 0.2s ease;

    &:hover {
      color: #0056b3;
    }

    &:focus {
      outline: none;
      text-decoration: underline;
    }
  }

  .contact-item,
  .company-item {
    font-size: 16px;
    color: #495057;
    margin-bottom: 12px;

    &:last-child {
      margin-bottom: 0;
    }

    strong {
      font-weight: 600;
      color: #212529;
    }
  }

  .website-link {
    color: #007bff;
    text-decoration: none;
    transition: color 0.2s ease;

    &:hover {
      color: #0056b3;
      text-decoration: underline;
    }
  }
}

// Modal transition animations
.modal-enter-active {
  transition: all 0.3s ease;
}

.modal-leave-active {
  transition: all 0.2s ease;
}

.modal-enter-from {
  opacity: 0;

  .modal-content {
    transform: scale(0.9);
  }
}

.modal-leave-to {
  opacity: 0;

  .modal-content {
    transform: scale(0.9);
  }
}

@media (max-width: 640px) {
  .modal-content {
    margin: 20px;
  }

  .modal-header {
    padding: 24px 24px 20px;

    .user-name {
      font-size: 24px;
    }
  }

  .modal-body {
    padding: 24px;
  }

  .info-section {
    margin-bottom: 24px;

    h3 {
      font-size: 18px;
    }
  }
}
</style>
