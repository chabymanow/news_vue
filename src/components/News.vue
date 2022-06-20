<template>
    <div class="newsSection">
        <h1>This is the news</h1>
        
            <div class="searchDiv">
                <div>
                    <label for="keyword">Search: </label>
                    <input class="keyword" name="keyword" type="text" v-model="keyWord" @keyup.enter="getData"/>
                </div>
                <div>
                    <label for="newsCategory">Select language: </label>
                    <select @change="changeLanguage" name="newsCategory">Choose category
                        <option value="en">English</option>
                        <option value="de">German</option>
                        <option value="es">Espanol</option>
                        <option value="fr">France</option>
                        <option value="he">Hebrew</option>
                        <option value="it">Italian</option>
                        <option value="nl">Dutch</option>
                        <option value="no">Norwegian</option>
                        <option value="pt">Portuguese</option>
                        <option value="ru">Russian</option>
                        <option value="sv">Swedish</option>
                        <option value="zh">Chinese</option>
                        <option value="ar">Arabic</option>
                    </select>
                </div>

                <div>
                    <label for="newsCategory">Order to: </label>
                    <select @change="changeOrder" name="newsOrder">
                        <option value="publishedAt">Published time</option>
                        <option value="relevancy">Relevancy</option>
                        <option value="popularity">Popularity</option>
                    </select>
                </div>
                <!-- This is for later TOP HEADLINES -->
                <!-- <div>
                    <label for="newsCategory">Select category:</label>
                    <select @change="changeCategory" name="newsCategory">Choose category
                        <option value="general">General</option>
                        <option value="entertainment">Entertainment</option>
                        <option value="business">Business</option>
                        <option value="health">Health</option>
                        <option value="science">Science</option>
                        <option value="sports">Sports</option>
                        <option value="technology">Technology</option>
                    </select>
                </div> -->
<!--                 
                <button @click="getData">
                    Get news
                    <i class="fa-solid fa-circle-arrow-right"></i>
                </button> -->
            </div>
        <div class="news">
            <div class="newsDiv">
            Fresh news about {{ keyWord }}
                <div class="articleDiv" v-show="freshNews" v-for="(news, index) in freshNews" :key="news.url">
                    <p class="author">Source:{{ news.source.name }} | Published: {{ news.publishedAt }}</p>  
                    <p @click="getNews(index)" class="newsTitle">{{ news.title }}</p>
                </div>
            </div>
            <div class="newsResult" v-show="newsDesc">
                <img class="newsImage" :src="newsImage" />
                <p class="author">Author:{{ author }} | Published: {{ date }}</p>
                <p class="newsDesc">{{ newsDesc }}</p>
                <a :href="newsLink" target="_blank">Go to page</a>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";

    export default {
        name: "NewsDiv",
        
        data(){
            return{
                keyWord: "world",
                myAPIkey: "GET_YOUR_API_KEY", // Get your API key at: https://newsapi.org/
                freshNews: [],
                newsDesc: "",
                newsImage: "",
                newsLink: "",
                newsLanguage: "en",
                newsOrder: "publishedAt",
                source: "",
                author: "",
                date: "",
                // newsCategory: "general",
                newsIndex: 0
            }
        },
        mounted(){            
            this.getData();
        },
        methods: {
            async getData(){
                
                const url="https://newsapi.org/v2/everything?q="+this.keyWord+"&language="+this.newsLanguage+"&sortBy="+this.newsOrder+"&apiKey="+this.myAPIkey;
                
                try{
                    const response = await axios.get(url);
                    this.freshNews = response.data.articles;
                    console.log(this.freshNews);
                }catch(e){
                    console.log(e);
                    alert("Error: "+e.response.status+"\nMessage: "+e.response.data.error.message);
                }
                //const results = response.data.results          
            },
            getNews(index){               
                this.newsDesc = this.freshNews[index].description;
                this.newsImage = this.freshNews[index].urlToImage;
                this.newsLink = this.freshNews[index].url;
                this.source = this.freshNews[index].source.name;
                this.author = this.freshNews[index].author;
                this.date = this.freshNews[index].publishedAt;
                if(this.fromDate < this.minDate){
                    this.fromDate == this.minDate;
                    document.getElementById('fromDate').valueAsDate = this.fromDate;
                }
                if(this.fromDate > this.maxDate){
                    this.fromDate == this.maxDate;
                    document.getElementById('fromDate').valueAsDate = this.fromDate;
                }
                window.scrollTo(0,0);
            },

            changeLanguage(e){
                this.newsLanguage =  e.target.value;
                this.getData();
            },
            changeOrder(e){
                this.newsOrder = e.target.value;
                this.getData();
            }
    }
}
</script>

<style scoped>

    input, select, select option{
        width: 15ch;
        font-size: 1.1em;
        font-weight: 300;
        background: #FFFFFF;
        border: none;
        border-bottom: 1px solid rgba(120, 120, 120, .5);
    }
    hr{
        width: 80%;
        border: 1px solid rgba(120, 120, 120, .5);
    }

    .newsSection{
        width: 80vw;
        margin: 0 auto;
    }
    .news{
        width: 90%;
        margin: 0 auto;
        display: flex;
        flex-direction: row;
    }

    .searchDiv{
        width: 90%;
        height: 70px;
        margin: 0 auto;
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: space-around;
        font-size: .9rem;
        font-weight: 700;
        margin-right: 3em;
        text-align: left;
        padding: .5rem 1rem;
    }

    .keyword{
        position:relative ;
    }
    .keyword::before{
        position: absolute;
        content: "QWW";
        inset: 0;
        width: 40px;
        height: 40px;
        background: rgb(255, 0, 0);
        z-index: 100;
    }

    .searchDiv button{
        width: 10rem;
        height: 40px;
        background: #6568f3;
        font-size: 1rem;
        font-weight: 400;
        color: #FAFAFA;
        border-radius: 10px;
    }
    .newsDiv{
        width: 25%;
        text-align: left;
        margin-right: 3em;
    }

    .articleDiv{
        border-bottom: 1px solid rgba(20, 20, 20, .2);
    }
    .author{
        font-size: .8rem;
    }
    .newsTitle{
        cursor: pointer;
    }

    .newsResult{
        width: 70%;
    }

    .newsResult a{
        text-decoration: none;
        background: #6568f3;
        font-size: 1.5rem;
        font-weight: 400;
        color: #FAFAFA;
        padding: .5em 1em;
        border-radius: 10px;

    }

    .newsImage{
        width: 100%;
        height: fit-content;
        margin: 0 auto;
        border-radius: 5px;
        margin-bottom: 1.5rem;
    }

    .newsDesc{
        width: 100%;
        text-align: left;
        font-size: 1.2rem;
        line-height: 150%;
    }

    .dateSelect{
        width: 50%;
        display: flex;
        flex-direction: column;
        gap: .3rem;
    }
</style>