<template>
    <v-container>
        <h1>{{ rocket?.rocket_name }}</h1>
        <v-card>
            <v-card-text>
                <p>{{ rocket?.description }}</p>
                <p>First flight date: {{ rocket?.first_flight }}</p>
                <p>Height: {{ rocket?.height.meters }}m</p>
                <p>Diameter: {{ rocket?.diameter.meters }}m</p>
                <p>Mass: {{ rocket?.mass.kg }}kg</p>
                <p>Number of stages: {{ rocket?.stages }}</p>
            </v-card-text>
        </v-card>
    </v-container>
</template>

<script lang="ts" setup>
interface Rocket {
    id: string
    rocket_name: string
    description: string
    first_flight: string
    height: { meters: number }
    diameter: { meters: number }
    mass: { kg: number }
    stages: number
}
const query = gql`
    query getRocket {
        rockets {
            name
            description
            first_flight
            height {
                meters
            }
            diameter {
                meters
            }
            mass {
                kg
            }
            stages
        }
    }
`

const rocketId = useRoute().params.id
const { result } = useQuery(query, {
    variables: { id: rocketId },
})

const rocket: Ref<Rocket | undefined> = ref()

if (result.value) {
    rocket.value = result.value.rocket
}
</script>
