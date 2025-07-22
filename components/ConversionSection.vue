<template>
  <section class="conversion">
    <h2 class="conversion__title">限时抢购 · 阶梯定价</h2>

    <div class="conversion__pricing">
      <div
        v-for="(item, index) in pricingSteps"
        :key="index"
        class="conversion__card"
      >
        <p class="conversion__card-title">{{ item.title }}</p>
        <p class="conversion__card-price">{{ item.price }}</p>
        <p class="conversion__card-desc">{{ item.description }}</p>
      </div>
    </div>

    <div class="conversion__countdown">
      距离结束还剩：<span class="conversion__highlight">{{ countdown }}</span>
    </div>

    <div class="conversion__slots">
      仅剩 <span class="conversion__highlight">{{ slotsLeft }}</span> 个名额
    </div>

    <button class="conversion__button" @click="showModal = true">
      立即抢占
    </button>

    <div v-if="showModal" class="modal">
      <div class="modal__content">
        <button class="modal__close" @click="showModal = false">✕</button>
        <p class="modal__title">扫码支付</p>
        <img
          class="modal__img"
          src="~/assets/img/zhanwei.jpg"
          alt="支付二维码"
        />
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from "vue";

const pricingSteps = [
  { title: "前100名", price: "¥99", description: "限前100名用户" },
  { title: "第101-300名", price: "¥129", description: "限时优惠" },
  { title: "第301名起", price: "¥159", description: "恢复原价" },
];

const countdown = ref("");
const showModal = ref(false);
const slotsLeft = ref(150);

const updateCountdown = () => {
  const endTime = new Date(Date.now() + 72 * 60 * 60 * 1000);
  const timer = setInterval(() => {
    const now = new Date();
    const diff = endTime - now;
    if (diff <= 0) {
      countdown.value = "00:00:00";
      clearInterval(timer);
      return;
    }
    const h = String(Math.floor(diff / 3600000)).padStart(2, "0");
    const m = String(Math.floor((diff % 3600000) / 60000)).padStart(2, "0");
    const s = String(Math.floor((diff % 60000) / 1000)).padStart(2, "0");
    countdown.value = `${h}:${m}:${s}`;
  }, 1000);
};

const simulateSlots = () => {
  setInterval(() => {
    if (slotsLeft.value > 10) {
      slotsLeft.value -= Math.floor(Math.random() * 3);
    }
  }, 8000);
};

onMounted(() => {
  updateCountdown();
  simulateSlots();
});
</script>

<style lang="scss" scoped>
.conversion {
  background-color: #fff8e1;
  padding: 60px 20px;
  text-align: center;

  &__title {
    font-size: 24px;
    font-weight: bold;
    margin-bottom: 30px;
    @media (max-width: 480px) {
      font-size: 20px;
    }
  }

  &__pricing {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 20px;
    margin-bottom: 30px;
    @media (max-width: 768px) {
      flex-direction: column;
      align-items: center;
    }
  }

  &__card {
    background: white;
    border-radius: 12px;
    padding: 20px;
    width: 200px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);

    &-title {
      font-weight: 600;
    }

    &-price {
      color: #e53935;
      font-size: 22px;
      font-weight: bold;
      margin-top: 10px;
    }

    &-desc {
      font-size: 12px;
      color: #888;
      margin-top: 5px;
    }
  }

  &__countdown,
  &__slots {
    margin: 10px 0;
    font-size: 16px;
    color: #555;
    @media (max-width: 480px) {
      font-size: 14px;
    }
  }

  &__highlight {
    font-weight: bold;
    color: #e53935;
  }

  &__button {
    background-color: #e53935;
    color: white;
    border: none;
    padding: 12px 24px;
    font-size: 16px;
    border-radius: 30px;
    margin-top: 20px;
    cursor: pointer;
    @media (max-width: 480px) {
      width: 100%;
      font-size: 15px;
    }
  }
}

.modal {
  position: fixed;
  inset: 0;
  background-color: rgba(0, 0, 0, 0.6);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;

  &__content {
    background: white;
    padding: 30px 20px;
    border-radius: 12px;
    width: 300px;
    position: relative;
    text-align: center;
    @media (max-width: 480px) {
      width: 90%;
    }
  }

  &__title {
    font-size: 18px;
    font-weight: bold;
    margin-bottom: 20px;
  }

  &__close {
    position: absolute;
    top: 10px;
    right: 14px;
    background: none;
    border: none;
    font-size: 20px;
    cursor: pointer;
  }

  &__img {
    max-width: 100%;
    height: auto;
  }
}
</style>
