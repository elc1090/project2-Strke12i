<template>
    <div class="flex items-center justify-center mx-auto w-1/2 mt-5">
        <div class="bg-slate-200 rounded-md grid grid-cols-4 gap-1 grid-flow-row grid-">
            <div class="p-5 text-center col-start-1 col-end-3 row-start-1 row-end-3">
                <img :src="data.images[1].url" alt="Band" class="h-32 rounded-full">
            </div>
            <div class="p-1 pt-4 col-start-3 col-end-5">
                <a v-bind:href="data.uri">
                    <p class="text-xl font-semibold hover:text-blue-400"> {{ data.name }}</p>
                </a></div>
            <div class="p-1 ">{{ data.genres[0] }}</div>
            <div class="p-1"> POP: {{data.popularity}}</div>
           
        </div>
    </div>
</template>
  
<script>
export default {
  props: ['data'],
};
</script>