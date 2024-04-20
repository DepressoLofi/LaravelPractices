<script setup>
import{computed } from 'vue';
import moment from 'moment';

const props = defineProps(['message']);
const formattedDate = computed(()=> {
    return moment(props.message.created_at).format('h:mm A');
});

</script>

<template>
    <div class="flex my-1 " :class="{'justify-end': $page.props.auth.user.id == message.user.id}">
        <div v-if="$page.props.auth.user.id == message.user.id" class="rounded py-2 px-3 bg-yellow-300">
            <p class="text-sm text-red-700 text-right">
                {{ message.user.name }}
            </p>
            <p class="text-sm mt-1">
                {{ message.message }}
            </p>
            <p class="text-right text-xs text-grey-600 mt-1">
                {{ formattedDate }}
            </p>
        </div>

        <div v-if="$page.props.auth.user.id != message.user.id" class="rounded py-2 px-3 bg-slate-300">
            <p class="text-sm text-teal-700">
                {{ message.user.name }}
            </p>
            <p class="text-sm mt-1">
                {{ message.message }}
            </p>
            <p class="text-right text-xs text-grey-600 mt-1">
                {{ formattedDate }}
            </p>

        </div>
    </div>
    
</template>

