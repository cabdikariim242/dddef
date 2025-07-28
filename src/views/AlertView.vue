<template>

    <BaseCard class="m-10">
    <p class="text-left mt-3">Lorem ipsum dolor, sit amet consectetur adipisicing elit. At voluptas vero, inventore esse amet, doloremque soluta incidunt alias ut quaerat nemo, tempore quo! Inventore dolor animi amet quia, assumenda nisi?</p>
        <h1 class=" text-left mt-2 mb-4">preview alert page examles</h1>
<div class="border-2 border-gray-300 p-4 rounded-lg">
          <div class="flex flex-col justify-start items-start mb-6  ">
        <alertComp :CustomClass="' text-white bg-red-500 p-4 rounded w-full text-left hover:shadow-lg transition-all duration-300  rounded-[14px] '" content="Awesome simple alert "/>
      </div>
            <div class="flex flex-col justify-start items-start mb-6 ">
        <alertComp :CustomClass="'bg-blue-500 p-4 rounded w-full text-left hover:shadow-lg transition-all duration-300  rounded-[14px] '" content="Awesome simple alert "/>
      </div>
            <div class="flex flex-col justify-start items-start mb-6 ">
        <alertComp :CustomClass="'text-indigo-700 bg-green-500 p-4 rounded w-full text-left hover:shadow-lg transition-all duration-300   rounded-[14px]'" content="Awesome simple alert "/>
      </div>
        <div class="flex flex-col justify-start items-start mb-6 ">
        <alertComp :CustomClass="'text-red bg-yellow-500 p-4 rounded w-full text-left hover:shadow-lg transition-all duration-300  rounded-[14px] '" content="Awesome simple alert"/>
      </div>
        <div class="flex flex-col justify-start items-start mb-6 ">
        <alertComp :CustomClass="'bg-pink-500 p-4 rounded w-full text-left hover:shadow-lg transition-all duration-300  rounded-[14px] '" content="Awesome simple alert"/>
      </div>
        <div class="flex flex-col justify-start items-start mb-6 ">
        <alertComp :CustomClass="' text-white bg-gray-500 p-4 rounded w-full text-left hover:shadow-lg transition-all duration-300  rounded-[14px]'" content="Awesome simple alert"/>
      </div>
        <div class="flex flex-col justify-start items-start  ">
        <alertComp :CustomClass="' text-black bg-cyan-500 p-4 rounded w-full text-left hover:shadow-lg transition-all duration-300  rounded-[14px] '" content="Awesome simple alert"/>
      </div>

</div>
 <p class="text-left mt-4">Lorem ipsum dolor sit, amet consectetur adipisicing elit. Itaque ullam, possimus alias quidem eaque illo dolor esse. Voluptate ducimus quas illo debitis ipsam modi tenetur dignissimos voluptatibus a soluta accusamus aperiam similique, velit repellat at animi sint sed id voluptates libero quia maiores! Iusto quidem consequatur animi at beatae quam labore, aliquam veniam dolor perferendis earum, quos vel dolores minus repudiandae excepturi eveniet, eos odio! Aspernatur qui impedit voluptatum provident ullam, nihil reiciendis. Porro labore modi provident deleniti iste tempore reiciendis aliquam ea dolores, at quis eveniet cum maxime veniam.</p>
</BaseCard>
<!-- second alert -->
    <BaseCard class="m-10">
            <p class="text-left mt-3">Lorem ipsum dolor, sit amet consectetur adipisicing elit. At voluptas vero, inventore esse amet, doloremque soluta incidunt alias ut quaerat nemo, tempore quo! Inventore dolor animi amet quia, assumenda nisi?</p>
        <h1 class=" text-left mt-2 mb-4">preview alert page examles</h1>
        <div class="border-2 border-gray-300 p-4 rounded-lg">
        <div class="flex items-center space-x-2">
            <alertComp icon="fa-solid fa-triangle-exclamation" :CustomClass="' flex justify-start items-center gap-2  text-black bg-cyan-500 p-4 rounded w-full text-left hover:shadow-lg transition-all duration-300  rounded-[14px] '" content="Awesome simple alert"/>
            </div>
            </div>
          </BaseCard>
              <BaseCard class="m-10">
            <p class="text-left mt-3">Lorem ipsum dolor, sit amet consectetur adipisicing elit. At voluptas vero, inventore esse amet, doloremque soluta incidunt alias ut quaerat nemo, tempore quo! Inventore dolor animi amet quia, assumenda nisi?</p>
        <h1 class=" text-left mt-2 mb-4">preview alert page examles</h1>
        <div class="border-2 border-gray-300 p-4 rounded-lg">
        <div class="flex items-center space-x-2">
            <alertComp   @cancelAlert="iscanceled[0] = false"
            v-if="iscanceled[0]" icon="fa-solid fa-triangle-exclamation" :CustomClass="' flex justify-start items-center gap-2  text-black bg-cyan-500 p-4 rounded w-full text-left hover:shadow-lg transition-all duration-300  rounded-[14px] '" content="Awesome simple alert"/>
            </div>
            </div>
          </BaseCard>
</template>

<script>
import BaseCard from '@/components/BaseCard.vue';
import AlertComp from '@/components/AlertComp.vue';
export default {
  components: {
    BaseCard,
    AlertComp
  },
    data() {
    return {
      index: 0,
      content: "Awesome simple alert",
      Content: "",
      CustomClass: "",
      spanClass: "",
      icon: "",
      iscanceled: [true],
    };
  },
  
}
  </script>