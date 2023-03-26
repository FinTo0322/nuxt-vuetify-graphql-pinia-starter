<template>
    <v-container>
        <h1>All SpaceX Launches</h1>
        <v-container>
            <v-text-field v-model="filterYear" label="Filter by Year" outlined></v-text-field>
            <v-select
                v-model="sortOrder"
                label="Sort by Launch Date"
                :items="['Ascending', 'Descending']"
            ></v-select>
            <ul>
                <li v-for="launch in sortedLaunches" :key="launch.id">
                    <h2>{{ launch.mission_name }}</h2>
                    <p v-if="data">Launch Date: {{ launch.launch_date_local }}</p>
                    <p v-if="data">
                        Launch Site: {{ launch.launch_site ? launch.launch_site.site_name_long : 'Unknown' }}
                    </p>

                    <nuxt-link :to="{ name: 'rocket', state: { id: launch.rocket.rocket_id } }">
                        {{ launch.rocket.rocket_name }}
                    </nuxt-link>
                    <p v-if="data && launch.details">Details: {{ launch.details }}</p>
                </li>
            </ul>
        </v-container>
    </v-container>
</template>

<script lang="ts" setup>
const query = gql`
    query getLaunches {
        launches {
            id
            mission_name
            launch_date_local
            launch_site {
                site_name_long
            }
            rocket {
                rocket_name
            }
            details
        }
    }
`

const filterYear = ref<number | null>(null)
const sortOrder = ref<'Ascending' | 'Descending'>('Ascending')

const { data } = useAsyncQuery<{
    launches: {
        id: string
        mission_name: string
        launch_date_local: string
        launch_site: {
            site_name_long: string
        }
        rocket: {
            rocket_id: string
            rocket_name: string
        }
        details?: string
    }[]
}>(query, { year: null })

const sortedLaunches = computed(() => {
    if (data.value && data.value.launches) {
        const launches = [...data.value.launches]
        if (sortOrder.value === 'Ascending') {
            launches.sort(
                (a, b) => new Date(a.launch_date_local).getTime() - new Date(b.launch_date_local).getTime(),
            )
        } else {
            launches.sort(
                (a, b) => new Date(b.launch_date_local).getTime() - new Date(a.launch_date_local).getTime(),
            )
        }
        return launches.filter((launch: any) => {
            return filterYear.value ? launch.launch_date_local.includes(filterYear.value.toString()) : true
        })
    } else {
        return []
    }
})
</script>
