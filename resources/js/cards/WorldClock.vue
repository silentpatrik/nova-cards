<template>

    <LoadingCardWithButton
        :heading="card.title || __('World Clock')"
        :card="card"
        :loading="loading"
        :loadingType="loadingType"
        :polling="polling"
        @update:polling="polling = $event"
        @refresh="fetch('button')"
    >
        <table v-if="data.length > 0" class="w-full text-left table-collapse">
            <tbody class="align-baseline">
                <tr v-for="time in data" :key="time.name">
                    <td class="py-2 pr-2 font-bold">
                        {{ time.name }}
                    </td>
                    <td class="p-2">
                        {{ time.time }}
                    </td>
                    <td class="p-2">
                        <Icon class="text-primary-500" :type="time.night ? 'moon' : 'sun'"></Icon>
                    </td>
                </tr>
            </tbody>
        </table>

        <div v-else>
            {{ __('No Data') }}
        </div>
    </LoadingCardWithButton>
</template>

<script>
    import Polling from '../mixins/Polling.js'

    export default {
        props: {
            card: {
                type: Object,
                required: true,
            },
        },

        mixins: [Polling],

        data: () => ({
            data: [],
        }),

        methods: {
            endpoint() {
                return Nova.request().get('/nova-vendor/stepanenko3/nova-cards/world-clock', {
                    params: {
                        timezones: this.card.timezones,
                        timeFormat: this.card.timeFormat,
                    },
                });
            },

            success(data) {
                this.data = data;
            },
        },
    }
</script>
