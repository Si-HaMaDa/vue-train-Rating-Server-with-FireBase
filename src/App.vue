<template>
    <learning-survey
        @reloadRatings="reloadRatings = !reloadRatings"
    ></learning-survey>
    <user-experiences :reloadRatings="reloadRatings"></user-experiences>
</template>

<script>
import LearningSurvey from "./components/survey/LearningSurvey.vue";
import UserExperiences from "./components/survey/UserExperiences.vue";

export default {
    components: {
        LearningSurvey,
        UserExperiences,
    },
    data() {
        return {
            reloadRatings: false,
        };
    },
    methods: {},
};
</script>

<style>
* {
    box-sizing: border-box;
}

html {
    font-family: sans-serif;
}

body {
    margin: 0;
}

.text-error::before {
    content: "*";
}

.text-error {
    color: red;
    font-weight: 600;
}
</style>
