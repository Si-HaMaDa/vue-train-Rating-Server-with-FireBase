<template>
    <section>
        <base-card>
            <h2>Submitted Experiences</h2>
            <div>
                <base-button @click="loadRatings">
                    Load Submitted Experiences
                </base-button>
            </div>
            <p v-if="isLoading">Loading...</p>
            <p v-else-if="!isLoading && error" class="text-error">
                {{ error }}
            </p>
            <p v-else-if="!isLoading && (!results || results.length === 0)">
                No stored experiences found. Start adding some survey results
                first.
            </p>
            <ul v-else>
                <survey-result
                    v-for="result in results"
                    :key="result.id"
                    :name="result.name"
                    :rating="result.rating"
                ></survey-result>
            </ul>
        </base-card>
    </section>
</template>

<script>
import SurveyResult from "./SurveyResult.vue";

export default {
    props: ["reloadRatings"],
    components: {
        SurveyResult,
    },
    data() {
        return {
            results: [],
            isLoading: false,
            error: null,
        };
    },
    watch: {
        reloadRatings() {
            this.loadRatings();
        },
    },
    methods: {
        loadRatings() {
            this.isLoading = true;
            this.error = null;
            fetch(
                "https://vue-train-rating-server-default-rtdb.europe-west1.firebasedatabase.app/rating.json"
            )
                .then((res) => {
                    if (!res.ok) throw new Error(res.statusText);

                    return res.json();
                })
                .then((data) => {
                    this.isLoading = false;
                    const results = [];
                    for (const id in data) {
                        results.push({
                            id: id,
                            name: data[id].name,
                            rating: data[id].rating,
                        });
                    }
                    this.results = results.reverse();
                })
                .catch((error) => {
                    this.isLoading = false;
                    this.error = error.message;
                });
        },
    },
    mounted() {
        this.loadRatings();
    },
};
</script>

<style scoped>
ul {
    list-style: none;
    margin: 0;
    padding: 0;
}
</style>
