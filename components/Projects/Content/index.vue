<template>
  <div>
    <div
      class="relative flex flex-col gap-7 md:gap-14 z-10"
      :class="containerWidth"
    >
      <DesignShapesBlob
        class="absolute blur-[200px] w-96 inset-x-0 mx-auto -top-52 -z-10 opacity-50"
      />

      <div class="flex flex-col gap-y-5 px-5">
        <h1 :class="subHeading2">Foundation Projects</h1>
        <div
          class="relative flex items-center mx-1 overflow-hidden min-h-96 h-full"
        >
          <button
            @click="scrollLeft"
            class="group absolute left-0 active:outline outline-accent rounded-2xl hidden md:inline"
          >
            <PhCaretLeft
              class="group-hover:text-accent w-6 h-6"
              :class="transition"
            />
          </button>
          <button
            @click="scrollRight"
            class="group absolute right-0 active:outline outline-accent rounded-2xl hidden md:inline"
          >
            <PhCaretRight
              class="group-hover:text-accent w-6 h-6"
              :class="transition"
            />
          </button>
          <div
            ref="scrollContainer"
            class="menu-scroll relative flex justify-start items-center h-fit gap-4 sm:gap-10 md:gap-20 px-2.5 overflow-x-scroll scroll-smooth mx-5 md:mx-14"
          >
            <HomeContentOurProjectsCard
              class="flex-none group"
              v-for="cardItem in Fdata"
              :title="cardItem.title"
              :description="cardItem.description"
              :funded="cardItem.funded"
              :target="cardItem.target"
              :completed="cardItem.completed"
              :image="cardItem.mediaFiles[0].url"
              :path="`${cardItem.title}`"
            >
            </HomeContentOurProjectsCard>
          </div>
        </div>
      </div>

      <div class="hidden flex-col gap-y-5 px-5">
        <h1 :class="subHeading2">Other Projects</h1>
        <div v-if="data && data.length" class="min-h-full">
          <div
            class="relative fle flex-wrap-reverse w-full overflow-hidden px-5 mx-auto gap-y-7 hidden"
          >
            <SubNavFilter />
            <div class="relative group basis-3/5 md:basis-2/5">
              <input
                type="search"
                name="search"
                id="search"
                placeholder="Search..."
                class="bg-slate-300/20 px-7 py-2.5 rounded-3xl outline-none w-full"
              />
              <PhMagnifyingGlass
                class="absolute inset-0 left-0 top-0 w-4 h-4 my-auto ml-2 text-slate-500/60"
              />
            </div>
            <ButtonOutline name="Filter" class="w-fit ml-auto">
              <IconsSymbolsFilter />
            </ButtonOutline>
          </div>
          <div
            class="relative flex flex-wrap justify-center gap-y-1.5 sm:gap-y-5 lg:gap-y-8 gap-x-1.5 sm:gap-x-5 md:gap-x-10 lg:gap-x-1 lg:justify-between h-fit"
          >
            <HomeContentOurProjectsCard2
              class="flex-none group"
              v-for="cardItem in data"
              :title="cardItem.title"
              :description="cardItem.description"
              :funded="cardItem.funded"
              :target="cardItem.target"
              :completed="cardItem.completed"
              :image="cardItem.image"
              :path="`/projects/p/${cardItem.title}`"
            >
            </HomeContentOurProjectsCard2>
          </div>
        </div>

        <div v-else class="flex justify-center items-center h-96">
          <h1 class="text-accent" :class="subHeading">No Project</h1>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import IconCompleted from "@/components/Icons/Projects/CompletedStatus.vue";
import {
  PhCaretRight,
  PhCaretLeft,
  PhMagnifyingGlass,
} from "@phosphor-icons/vue";
const { containerWidth, subHeading2, transition, subHeading } =
  useTailwindConfig();
const isLoading = ref(false);
const { getProjects, getFProjects } = useProject();
const data: any = ref([]);
const Fdata: any = ref([]);

onBeforeMount(async () => {
  isLoading.value = true;
  try {
    const { projects }: any = await getProjects();
    const { Fprojects }: any = await getFProjects();
    data.value = projects;
    Fdata.value = Fprojects;
  } catch (error) {
    console.log(error);
  } finally {
    isLoading.value = false;
  }
});

const scrollContainer = ref<HTMLDivElement | null>(null);

const scrollRight = () => {
  if (scrollContainer.value) {
    scrollContainer.value.scrollBy({ left: 400, behavior: "smooth" });
  }
};

const scrollLeft = () => {
  if (scrollContainer.value) {
    scrollContainer.value.scrollBy({ left: -400, behavior: "smooth" });
  }
};
</script>
<style>
/* Target the cancel button */
input[type="search"]::-webkit-search-cancel-button {
  /* Override default appearance */
  -webkit-appearance: none; /* Safari and Chrome */
  appearance: none; /* Other browsers */
  /* Add your custom styles here */
  background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor"><path opacity="0.3" fill-rule="evenodd" d="M5.293 5.293a1 1 0 011.414 0L10 8.586l3.293-3.293a1 1 0 111.414 1.414L11.414 10l3.293 3.293a1 1 0 01-1.414 1.414L10 11.414l-3.293 3.293a1 1 0 01-1.414-1.414L8.586 10 5.293 6.707a1 1 0 010-1.414z" clip-rule="evenodd"/></svg>');
  background-size: 1.25rem; /* Adjust icon size */
  background-repeat: no-repeat;
  background-position: center;
  padding: 0.25rem;
  cursor: pointer;
  border: none; /* Remove border */
  outline: none; /* Remove focus outline */
}
</style>
