<template>
    <div>
        <div>
            <SearchBar  @termChange="onTermChange"/>
            <VideoDetail :video="selectedVideo"/>
            <VideoList :videos="videos" @videoSelect="onVideoSelect" />
            
        </div>
    </div>
</template>


<script>
import axios from 'axios';
import SearchBar from "./components/SearchBar";
import VideoList from "./components/VideoList";
import VideoDetail from "./components/VideoDetail";
const API_KEY = process.env.VUE_APP_API_KEY; //Youtube APIKEY
export default {
    name:'App',
    components:{
        SearchBar,
        VideoList, 
        VideoDetail
    },
    props:['video'],
    data(){
        return{videos:[], selectedVideo:null};
    },
    methods:{
        onTermChange(searchTerm){
            axios.get('https://www.googleapis.com/youtube/v3/search',{
                params:{
                    key:API_KEY,
                    type:'video',
                    part:'snippet',
                    q:searchTerm
                }
            })
            .then(response =>{
                this.videos = response.data.items;
            })
        },
        onVideoSelect(video){
            this.selectedVideo = video
        }
    }
}
</script>