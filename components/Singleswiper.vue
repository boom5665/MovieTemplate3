<template>
    <div class="single-movie-block">
        <div class="poster-container">
            <nuxt-img class="poster" :loading="_fetchMode" :src="_obj.imageslide" height="1000" :alt="_obj.full_name" />

            <div class="poster-overlay">
                <nuxt-link :to="getPlayUrl(_obj)">
                    <nuxt-img  format="webp" src="/play.png" alt="loader" class="img-play" />
                </nuxt-link>
                <!-- <div class="rating" v-show="_obj.ratescore">{{ score ? score : _obj.ratescore }}</div> -->
                <div class="title">{{ _obj.full_name }}</div>
                <div class="des">{{ _obj.description }}</div>
            </div>
        </div>

        <!-- <div class="d-flex align-items-center">
            <div class="resolution">{{ _obj.quality }}</div>
            <div class="sound flex-grow-1" v-show="_obj.sound_main">เสียง : {{ _obj.sound_main }}</div>
            <div class="view"><b-icon-eye class="view-icon" /> {{ _obj.view }}</div>
        </div> -->
    </div>
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

<style lang="scss" scoped>
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
.img-play {
    top: 40%;
    position: absolute;
    left: 47%;
    width: 70px;
    @media screen and (max-width: 576px) {
        top: 30%;
        position: absolute;
        left: 40%;
        width: 70px;
    }
}
</style>
