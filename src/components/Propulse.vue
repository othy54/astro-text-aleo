<template>
  <div class="lg:grid lg:grid-cols-12 lg:gap-8">
    <div class="lg:col-span-5 lg:order-2 lg:col-start-7 xl:pt-10">
      <h2 class="section-com__title">
        Propulse ta visibilité et atteins tes objectifs
      </h2>
      <!-- <LazySliderHome
        v-if="!isLargeThanLg"
        class="mt-8"
        :datas="dataPropulse"
      /> -->
      <ul class="mt-6">
        <li
          v-for="(propulse, i) in data"
          :key="'propulse-' + i"
          class="section-com__list-item"
          @mouseenter="pausePropulse(i)"
          @mouseleave="animationPropulse()"
        >
          <div class="section-com__spin">
            <svg
              width="24"
              height="24"
              viewBox="0 0 24 24"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <circle
                cx="12"
                cy="12"
                r="10.5"
                transform="rotate(-90 12 12)"
                stroke="#280837"
                stroke-width="3"
              />
              <circle
                cx="12"
                cy="12"
                r="10.5"
                transform="rotate(-90 12 12)"
                stroke="#A2CCEE"
                stroke-width="3"
                class="spin-progress"
                :class="'spin-progress-' + i"
              />
            </svg>
          </div>
          <div>
            <h3 class="section-com__list-title">
              {{ propulse.title }}
            </h3>
            <p
              class="section-com__list-text"
              :class="[
                propulse.isActive ? 'section-com__list-text--collapse' : '',
              ]"
            >
              {{ propulse.text }}
            </p>
          </div>
        </li>
      </ul>
    </div>
    <div class="lg:col-span-5 lg:col-start-2 lg:order-1 xl:pr-[56px]">
      <div class="section-com__image-wrapper">
        <div ref="lottieWrapper"></div>
      </div>
    </div>
  </div>
</template>
<script >
import { onMounted, ref, reactive, nextTick } from "vue";
import gsap from "gsap";
import lottie from "lottie-web";

export default {
  setup() {
    const data = ref([
      {
        title: "Améliore ton image",
        text: "Améliorer la manière dont tes clients te perçoivent te permettra de sortir du lot et de dépasser la concurrence.",
        isActive: true,
        file: "anim-ameliore-image",
      },
      {
        title: "Multiplie tes contacts",
        text: "Les demandes de devis sont le moteur du développement commercial de ton entreprise. Avec ALEO, génère un maximum d’opportunités.",
        isActive: false,
        file: "anim-multiplie-contacts",
      },
      {
        title: "Augmente ta notoriété",
        text: "Une bonne notoriété est indispensable pour ton business. Alors sois présent et actif pour marquer les esprits de tes futurs clients !",
        isActive: false,
        file: "anim-notoriete",
      },
      {
        title: "Fidélise tes clients",
        text: "Loin des yeux, loin du cœur… Noue une relation durable et de confiance avec ta clientèle grâce à une communication régulière et des actions ciblées.",
        isActive: false,
        file: "anim-fidelisation",
      },
    ]);

    const animation = ref({});
    const step = ref(0);
    const lottieWrapper = ref(null);

    const options = reactive({
      container: lottieWrapper,
      renderer: "svg",
      autoplay: true,
      loop: false,
      path: `/${data.value[step.value].file}.json`,
    });

    const animate = () => {
      animation.value = gsap.fromTo(
        ".spin-progress-" + step.value,
        {
          css: {
            strokeDashoffset: "66px",
          },
        },
        {
          duration: 5,
          ease: "none",
          overwrite: true,
          css: {
            strokeDashoffset: "0px",
          },
          onComplete: () => {
            animation.value.pause(0).kill();
            data.value[step.value].isActive = false;
            step.value++;
            if (step.value > 3) {
              step.value = 0;
            }
            data.value[step.value].isActive = true;
            options.path = `/${data.value[step.value].file}.json`;
            lottie.destroy();
            nextTick(() => {
              lottie.loadAnimation(options);
            });
            animate();
            // this.forceRerender();
          },
        }
      );
    };

    onMounted(() => {
      lottie.loadAnimation(options);
      animate();
    });

    return {
      data,
      animation,
      animate,
      options,
      lottieWrapper,
    };
  },
};
</script>
<style lang="postcss">
.section-com {
  @apply text-accent-200;

  &__title {
    @apply text-accent-500;

    margin-top: 16px;
    margin-bottom: 16px;
    font-style: italic;
    font-weight: 900;
    font-size: 32px;
    line-height: 40px;

    @screen xl {
      font-size: 40px;
      line-height: 44px;
    }
  }

  &__list {
    &-item {
      margin-bottom: 24px;
      display: flex;
      gap: 16px;
      cursor: pointer;
    }

    &-title {
      font-style: italic;
      font-weight: 900;
      font-size: 20px;
      line-height: 24px;
    }

    &-text {
      font-style: normal;
      font-weight: 500;
      font-size: 16px;
      line-height: 24px;
      margin-top: 8px;
      max-height: 0;
      overflow: hidden;
      transition: 0.5s;
      opacity: 0;

      &--collapse {
        max-height: 70px;
        opacity: 1;
      }
    }
  }

  &__image-wrapper {
    border-radius: 24px;
    padding: 80px;
    background: rgba(162, 204, 238, 0.1);
    box-shadow: inset 1px 1px 4px rgba(162, 204, 238, 0.4);

    & > img {
      width: 100%;
    }
  }
}

.spin-progress {
  stroke-dasharray: 66px;
  stroke-dashoffset: 66px;
}
</style>
