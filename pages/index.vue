<template>
  <main class="container max-w-sm px-4 py-12 mx-auto">
    <section
      v-for="(resource, resourceKey) in resources"
      :key="`resource-${resourceKey}`"
      class="mb-12"
    >
      <header class="mb-6 text-center text-gray-800">
        <span class="px-4 py-2 leading-5 bg-orange-50">{{
          resource.title
        }}</span>
        <h2 class="block my-2 text-2xl font-extrabold leading-7 tracking-tight">
          {{ resource.label }}
        </h2>
        <p class="tracking-tight text-gray-500">
          {{ resource.description }}
        </p>
      </header>
      <article
        v-for="(item, itemKey) in resource.items"
        :key="`item-${itemKey}`"
        class="relative mb-4 last-of-type:mb-0"
      >
        <div v-if="!item?.images?.length" class="relative rounded-lg">
          <img
            v-if="!item?.images?.length"
            :src="item.avatar"
            :alt="item.title"
            class="object rounded-2xl aspect-[1080/1350]"
            width="1080"
            height="1350"
          />
          <div
            class="absolute top-0 bottom-0 left-0 right-0 rounded-lg bg-white/20 backdrop-blur mix-blend-normal"
          ></div>
          <div
            class="absolute top-0 bottom-0 left-0 right-0 flex flex-col items-center justify-center p-4 text-base leading-5 text-center rounded-lg bg-white/40"
          >
            <div class="relative rounded-full">
              <img
                :src="item.avatar"
                :alt="item.title"
                class="rounded-full"
                width="64"
                height="64"
              />
              <div
                :class="[
                  'absolute bg-[#0D8047] w-3 h-3 border-2 border-white rounded-full bottom-0 right-0',
                  {
                    hidden: !item.available,
                  },
                ]"
              >
                <span class="sr-only">Opend</span>
              </div>
            </div>
            <h3 class="mt-4 font-bold text-gray-800">
              {{ item.title }}
            </h3>
            <span class="text-gray-600">
              {{ item.description }}
            </span>
          </div>
        </div>
        <div v-else :class="['relative rounded-2xl bg-white/80']">
          <img
            v-for="(image, imageKey) in item.images"
            :key="`image-${imageKey}`"
            :src="image.src"
            :alt="image.alt"
            class="rounded-2xl aspect-[1080/1350]"
            width="1080"
            height="1350"
          />
        </div>
        <NuxtLink
          :href="`/${item.slug}/`"
          :class="[
            'absolute inline-flex items-center gap-2 p-2 m-1 transition-all ease-in-out bottom-1 left-1 right-1 backdrop-blur rounded-xl bg-gray-800/80 border-t-white/20 ring-3 ring-transparent hover:ring-white/25 hover:bg-gray-800/90',
            { 'text-center justify-center py-5': !item?.images?.length },
          ]"
        >
          <div v-if="!!item?.images?.length" class="relative rounded-full">
            <img
              :src="item.avatar"
              :alt="item.title"
              class="rounded-full"
              width="40"
              height="40"
            />
            <div
              :class="[
                'absolute bg-[#0D8047] w-3 h-3 border-2 border-white rounded-full bottom-0 right-0',
                {
                  hidden: !item.available,
                },
              ]"
            >
              <span class="sr-only">Opend</span>
            </div>
          </div>
          <h3 class="text-base font-medium leading-5 text-white">
            {{ !item?.images?.length ? 'Ver mais' : item.title }}
          </h3>
        </NuxtLink>
      </article>
    </section>
    <Teleport to="body">
      <BaseModal
        v-model="shouldShowSwipeModal"
        contents-height="80vh"
        border-top-radius="16px"
        @update:model-value="onClose"
      >
        <BaseSingle :slug="slug" />
      </BaseModal>
    </Teleport>
  </main>
</template>

<script lang="ts" setup>
import { useRouteParams } from '@vueuse/router'

// type Provider = {
//   slug: string
//   title: string
//   avatar: string
//   available: boolean
//   images: {
//     src: string
//     alt: string
//   }[]
//   description: string
// }

// type Resource = {
//   slug: string
//   title?: string
//   label?: string
//   description?: string
//   items?: Provider[]
// }

const resources = [
  {
    type: 'hamburgueria',
    title: 'Hamburgueria',
    label: 'Del??cias de carne e sabores incr??veis! ????',
    description: 'Aqui algumas recomenda????es de hamburguerias na cidade',
    items: [
      ...Array(2).fill({
        slug: 'dom-catuto-burger',
        title: 'Dom Catulo Burguer',
        avatar: '/images/40x40.png',
        available: true,
        description:
          'Lorem ipsum dolor sit amet consectetur. Venenatis egestas.',
        images: Array(1).fill({
          src: '/images/1080x1350.png',
          alt: '',
        }),
      }),
      {
        slug: 'dom-catuto-burger2',
        title: 'Dom Catulo Burguer',
        avatar: '/images/40x40.png',
        available: true,
        description:
          'Lorem ipsum dolor sit amet consectetur. Venenatis egestas.',
      },
    ],
  },
  {
    type: 'doces-guloseimas',
    label: 'Del??cias de doces e guloseimas com sabores incr??veis! ????',
    title: 'Doces & Guloseimas',
    description: 'Aqui algumas recomenda????es de hamburguerias na cidade',
    items: [
      ...Array(2).fill({
        slug: 'dom-catuto-burger',
        title: 'Dom Catulo Burguer',
        avatar: '/images/40x40.png',
        available: true,
        description:
          'Lorem ipsum dolor sit amet consectetur. Venenatis egestas.',
        images: Array(1).fill({
          src: '/images/1080x1350.png',
          alt: '',
        }),
      }),
      {
        slug: 'dom-catuto-burger2',
        title: 'Dom Catulo Burguer',
        avatar: '/images/40x40.png',
        available: true,
        description:
          'Lorem ipsum dolor sit amet consectetur. Venenatis egestas.',
      },
    ],
  },
]

useHead({
  title: 'OndeVamos.app',
  meta: [
    {
      name: 'description',
      content:
        'Explore os melhores lugares e encontre a experi??ncia perfeita para voc??!',
    },
    {
      name: 'og:type',
      content: 'website',
    },
    {
      name: 'og:url',
      content: 'https://ondevamos.app/',
    },
    {
      name: 'og:title',
      content: 'OndeVamos.app',
    },
    {
      name: 'og:description',
      content:
        'Explore os melhores lugares e encontre a experi??ncia perfeita para voc??!',
    },
    {
      name: 'og:image',
      content: 'https://ondevamos.app/images/social.png',
    },
    {
      name: 'twitter:card',
      content: 'summary_large_image',
    },
    {
      name: 'twitter:url',
      content: 'https://ondevamos.app/',
    },
    {
      name: 'twitter:title',
      content: 'OndeVamos.app',
    },
    {
      name: 'twitter:description',
      content:
        'Explore os melhores lugares e encontre a experi??ncia perfeita para voc??!',
    },
    {
      name: 'twitter:image',
      content: 'https://ondevamos.app/images/social.png',
    },
  ],
})

// const route = useRoute()

const slug = useRouteParams('slug')

const shouldShowSwipeModal = ref(false || !!slug.value)

const router = useRouter()

watch(slug, (newSlug, _oldSlug) => {
  if (newSlug) {
    shouldShowSwipeModal.value = true
  }
})

const onClose = () => {
  return router.push({
    path: '/',
  })
}
</script>
