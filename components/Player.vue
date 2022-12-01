<template>
    <div class="player-page">
        <b-container>
            <b-row>
                <b-col cols="12" lg="8" xl="8" class="nopadding">
                    <div class="dis-between">
                        <h2 class="list-title">{{ _movieObj.full_name }}</h2>
                    </div>
                    <b-aspect aspect="16:9" class="mb-2">
                        <!-- <div class="iframe-container" style="z-index: 0;">
                            <iframe id="playervideo" :src="playerIframeUrl" allowfullscreen="allowfullscreen" style="width: 100%; height: 100%; border: 0px; overflow: hidden"></iframe>
                            <AdsVideo :_adsArray="adsVideoList" v-model="isShowAds" v-if="adsVideoList.length > 0 && isShowAds" />
                        </div> -->
                        <div class="iframe-container" style="z-index: 0 ; cursor: pointer;" @click="Movie()">
                            <nuxt-img format="webp" src="/playtem.png" alt="loader" />
                        </div>
                    </b-aspect>

                    <div class="d-flex align-items-center justify-content-between">
                        <div class="sound-container">
                            <div class="btn-bot" v-if="_movieObj.player_link_main != '' && currentSound != 'Main'" @click="currentSound = 'Main'">ลิ้งค์หลัก ({{ _movieObj.sound_main }})</div>
                            <div class="btn-bot" v-if="_movieObj.player_link_backup1 != '' && currentSound != 'Backup1'" @click="currentSound = 'Backup1'">
                                ลิ้งค์สำรอง 1 ({{ _movieObj.sound_backup1 }})
                            </div>
                            <div class="btn-bot" v-if="_movieObj.player_link_backup2 != '' && currentSound != 'Backup2'" @click="currentSound = 'Backup2'">
                                ลิ้งค์สำรอง 2 ({{ _movieObj.sound_backup2 }})
                            </div>
                        </div>
                        <div class="btn-bot-alert" @click="showReportPopup = true">แจ้งหนังเสีย</div>
                    </div>
                    <div class="custom-card-container mt-3" v-if="_type == 'series'">
                        <div class="custom-card-header">เลือกตอน</div>
                        <div class="custom-card-content">
                            <div class="ep-list">
                                <nuxt-link
                                    :to="getEpLink(index + 1)"
                                    class="ep-row"
                                    :class="{ active: index + 1 == _ep }"
                                    v-for="(value, index) in epArray"
                                    :key="index"
                                    v-show="index + 1 != epArray.length">
                                    EP {{ index + 1 }}
                                </nuxt-link>
                            </div>
                        </div>
                    </div>
                    <div class="popup-container" id="request-popup" v-show="showReportPopup" @click.self="showReportPopup = false">
                        <div class="popup-content">
                            <div class="text-right"><b-icon-x-circle-fill class="popup-close-btn" @click="showReportPopup = false" /></div>
                            <div class="tab-btn-list">
                                <div class="tab-btn active">แจ้งหนังเสีย</div>
                            </div>
                            <div class="tab-content-list">
                                <div class="tab-content">
                                    <textarea id="request-detail" style="height: 150px" class="popup-input" v-model="reportDetail" @keyup="isLetter"></textarea>
                                    <div class="text-center">
                                        <div class="submit-btn" @click="reportMovie()">ส่ง</div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="preview-header"><h2>ตัวอย่างหนัง</h2></div>
                    <div class="preview-container">
                        <div class="preview-poster">
                            <nuxt-img loading="lazy" format="webp" :src="_movieObj.picture" :alt="_movieObj.full_name" />
                        </div>
                        <div class="preview-url">
                            <div class="">{{ _isAV ? "ตัวอย่างหนัง:" : "" }} {{ _movieObj.full_name }}</div>
                            <iframe class="preview-movie" :src="previewUrl" frameborder="0" allowfullscreen></iframe>
                        </div>
                    </div>

                    <b-aspect aspect="16:6" class="mb-2">
                        <b-container>
                            <b-row>
                                <b-col cols="12" lg="12" xl="12" class="dis-colum-center">
                                    <div class="d-flex">
                                        <div class="player-top-content">
                                            <div>คะแนน :</div>
                                            <div class="player-top-content-font">{{ _movieObj.ratescore }} / {{ _isAV ? 100 : 10 }}</div>
                                        </div>
                                        <div class="player-top-content">
                                            <div>ปีที่ฉาย :</div>
                                            <div class="player-top-content-font">{{ _movieObj.year }}</div>
                                        </div>
                                        <div class="player-top-content">
                                            <div>เสียง :</div>
                                            <div class="player-top-content-font">{{ _movieObj.sound_main }}</div>
                                        </div>
                                        <div class="player-top-content">
                                            <div>ประเภท :</div>
                                            <div class="d-flex">
                                                <div class="player-top-content-font" style="margin-right: 5px" v-for="(value, index) in _movieObj.Category" :key="index">{{ value.name_th }}</div>
                                            </div>
                                        </div>
                                    </div>

                                    <div class="movie-description">
                                        <div><h4>เรื่องย่อ :</h4></div>
                                        {{ _movieObj.description }}
                                    </div>
                                </b-col>
                            </b-row>
                        </b-container>
                    </b-aspect>
                </b-col>
                <b-col cols="12" lg="4" xl="4" class="nopadding">
                    <b-container>
                        <div class="dis-between">
                            <h2 class="list-title">หนังแนะนำ</h2>
                        </div>
                        <b-row style="display: grid; grid-template-columns: 1fr 1fr">
                            <b-col style="margin-bottom: 10px" cols="12" lg="12" xl="12" v-for="(value, index) in movieList" :key="index">
                                <SingleMovielist :_obj="value" />
                            </b-col>
                        </b-row>
                    </b-container>
                </b-col>
            </b-row>
        </b-container>
        <div class="" v-if="movieList.length">
            <div class="dis-between">
                <h2 class="list-title">หนังแนะนำ</h2>
            </div>
            <div class="" v-if="movieList">
                <swipercate />
            </div>
            <div class="loader-container" v-show="loadingList">
                <nuxt-img format="webp" src="/loader.png" alt="loader" />
            </div>
        </div>

        <div class="loader-container fixed" v-show="showLoader">
            <nuxt-img format="webp" src="/loader.png" alt="loader" />
        </div>
    </div>
</template>

<script>
export default {
    props: {
        _id: {
            type: Number,
            required: true,
            default: 0,
        },
        _slug: {
            type: String,
            required: true,
            default: "",
        },
        _ep: {
            type: String,
            required: false,
            default: "",
        },
        _type: {
            type: String,
            required: false,
            default: "movie",
        },
        _movieObj: {
            type: Object,
            required: true,
        },
        _isAV: {
            type: Boolean,
            required: false,
            default: false,
        },
    },
    data() {
        return {
            currentSound: "Main",
            playerIframeUrl: "",
            serieLinkMain: "",
            epArray: [],
            relateList: [],
            showReportPopup: false,
            reportDetail: "",
            adsVideoList: [],
            isShowAds: true,
            showLoader: false,
            showmovie: true,
            movieList: [],
            currentPage: 1,
            maxPage: 1,
        };
    },
    computed: {
        previewUrl: function () {
            if (this._movieObj.preview) return "https://www.youtube.com/embed/" + this._movieObj.preview + "?controls=0&rel=0";
            return "";
        },
        catIdArray() {
            let cat = [];
            for (const key in this._movieObj.Category) {
                if (Object.hasOwnProperty.call(this._movieObj.Category, key)) {
                    const element = this._movieObj.Category[key];
                    cat.push(element.id);
                }
            }
            return cat;
        },
    },
    created() {
        this.setPlayerUrl();
        this.getAdsVideo();
        this.getRelateMovies();

        if (this._isHot) {
            this.getHotMovies();
        } else if (this._isSearch) {
            this.getSearchMovie();
        } else {
            this.getMovies();
        }
    },
    mounted() {},
    watch: {
        currentSound() {
            const self = this;
            this.playerIframeUrl = "";
            setTimeout(() => {
                self.setPlayerUrl();
            }, 100);
        },
    },
    methods: {
        showMovie() {
            if (this.showmovie) {
                this.showmovie = false;
            }
        },
        setPlayerUrl() {
            if (this._type == "series") {
                if (this.currentSound == "Main") this.epArray = this._movieObj.player_series_link_main.split("!!end!!");
                if (this.currentSound == "Backup1") this.epArray = this._movieObj.player_series_link_backup1.split("!!end!!");
                for (let index = 0; index < this.epArray.length; index++) {
                    const element = this.epArray[index];
                    if (index + 1 == this._ep) this.playerIframeUrl = "https://movie.aegistrex.com/api/player/" + element.split("}}")[1];
                }
            } else {
                if (this._isAV) {
                    this.playerIframeUrl = "https://movie.aegistrex.com/av/api/player/" + this._movieObj.player_link_main;
                } else {
                    if (this.currentSound == "Main") this.playerIframeUrl = "https://movie.aegistrex.com/api/player/" + this._movieObj.player_link_main;
                    if (this.currentSound == "Backup1") this.playerIframeUrl = "https://movie.aegistrex.com/api/player/" + this._movieObj.player_link_backup1;
                    if (this.currentSound == "Backup2") this.playerIframeUrl = "https://movie.aegistrex.com/api/player/" + this._movieObj.player_link_backup2;
                }
            }
        },
        getRelateMovies() {
            const self = this;
            this.$axios
                .$post("movie/listmoviesimilar", {
                    category: self.catIdArray,
                    perpage: 12,
                    page: 1,
                })
                .then(function (response) {
                    if (response.code == 200) {
                        self.relateList = response.result;
                    }
                });
        },
        getEpLink(ep) {
            return "/" + this._type + "/" + this._id + "/" + this._slug + "/" + ep;
        },
        Movie() {
            alert("กรุณาซื้อแพ็กเกจ");
        },
        reportMovie() {
            const detail = this.reportDetail;
            if (detail.trim() == "") {
                alert("กรุณากรอกข้อมูลให้ครบ");
                return;
            }
            // const self = this;
            // this.showLoader = true;
            // let type_id = 0;
            // if (this._isAV) type_id = 1;
            // this.$axios
            //     .$post("moviebroken", {
            //         movie_id: self._id,
            //         movie_type: type_id,
            //         reason: detail,
            //     })
            //     .then(function (response) {
            //         self.showReportPopup = false;
            //         self.showLoader = false;
            //         self.reportDetail = "";
            //         if (response.code == 200) {
            //             alert("สำเร็จ");
            //         }
            //     });
            alert("กรุณาซื้อแพ็กเกจ");
        },
        getHotMovies() {
            const self = this;
            this.loadingList = true;
            this.$axios
                .$post("moviehot", {
                    order: "ID",
                    orderby: "DESC",
                    types: "",
                    category: "",
                    year: 0,
                    perpage: 8,
                    page: this.currentPage,
                    search: this._search,
                })
                .then(function (response) {
                    if (response.code == 200) {
                        // console.log(response.result);
                        self.movieList = response.result;
                        self.maxPage = response.page_total;
                    }
                    self.loadingList = false;
                });
        },
        getMovies() {
            const self = this;
            this.loadingList = true;
            this.$axios
                .$post("movie/listmovie", {
                    order: "ID",
                    orderby: "DESC",
                    types: "",
                    category: "",
                    year: 0,
                    perpage: 8,
                    page: this.currentPage,
                    search: this._search,
                })
                .then(function (response) {
                    if (response.code == 200) {
                        // console.log(response.result);
                        self.movieList = response.result;
                        self.maxPage = response.page_total;
                    }
                    self.loadingList = false;
                });
        },
        getAdsVideo() {
            const self = this;
            this.$axios.$get("list/ads-video").then(function (response) {
                if (response.code == 200) {
                    self.adsVideoList = response.result ? response.result : [];
                }
            });
        },
    },
};
</script>
