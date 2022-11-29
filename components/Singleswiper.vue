<template>
    <nuxt-link :to="getPlayUrl(_obj)" class="single-movie-block">
        <div class="poster-container">
            <nuxt-img class="poster" :loading="_fetchMode" :src="_obj.picture" height="1000" :alt="_obj.full_name" />

            <div class="poster-overlay">
                <!-- <div class="rating" v-show="_obj.ratescore">{{ score ? score : _obj.ratescore }}</div> -->
                <!-- <b-icon-play-circle class="poster-play" /> -->
                <div class="title">{{ _obj.full_name }}</div>
                <div class="des">{{ _obj.description }}</div>
            </div>
        </div>

        <!-- <div class="d-flex align-items-center">
            <div class="resolution">{{ _obj.quality }}</div>
            <div class="sound flex-grow-1" v-show="_obj.sound_main">เสียง : {{ _obj.sound_main }}</div>
            <div class="view"><b-icon-eye class="view-icon" /> {{ _obj.view }}</div>
        </div> -->
    </nuxt-link>
</template>

<script>
export default {
    props: {
        _obj: {
            type: Object,
            required: true,
        },
        _fetchMode: {
            type: String,
            required: false,
            default: "lazy",
        },
    },
    data() {
        return {
            score: "",
        };
    },
    mounted() {
        this.getfixscore();
    },

    methods: {
        getfixscore() {
            this.score = Number.parseFloat(this._obj.ratescore).toFixed(1);
        },
    },
};
</script>

<style lang="css" scoped>
.poster {
    height: 500px !important;
}
.title {
    font-size: 30px;
}
.poster-overlay {
    display: flex;
    flex-direction: column !important;
    justify-content: flex-end !important;
    align-content: flex-start !important;
    align-items: flex-start !important;
}
.title {
    line-height: 30px;
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
    word-wrap: break-word;
    width: 100%;
    position: relative;
    z-index: 5;
    /* bottom: 55%; */
    padding: 10px;
    color: white;
}

.des {
    font-size: 12px;
    line-height: 30px;
    white-space: break-spaces;
    text-overflow: ellipsis;
    overflow: hidden;
    width: 70%;
    position: relative;
    z-index: 5;
    /* top: 45%; */
    left: 0;
    padding: 10px;
    color: white;
}
</style>
