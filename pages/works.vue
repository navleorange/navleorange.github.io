<style lang="scss" scoped>

#works_container{

    width: 100vw;
    height: 100vh;
    margin-top: 5%;

    @include mq(sp){
        margin-top: 20%;
        font-size: 10px;
    }

    .private_works_container, .research_works_container, .team_works_container{
        // 2列中央揃えでcontentの表示
        width: 100%;
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
    }

    h1{
        width: 90%;
        text-align: center;
        height: 3.5rem;
        display: flex;
        align-items: center;
        background-color: #8dd98d33;
        border-left: solid 5px yellowgreen;
        border-bottom: solid 2px #d7d7d7;/*下線*/
        padding-left: 7px;
    }

    h2{
        text-align: center;
    }

    h3{
        display: block;
        margin-left: auto;
        margin-right: auto;
        width: 70%;

        @include mq(sp){
            margin-left: 15px;
        }

        text-align: center;
        position: relative;
        height: 2.5rem;
        display: flex;
        align-items: center;
        background-color: #8dd98d33;
        border-left: solid 5px yellowgreen;
        border-bottom: solid 2px #d7d7d7;/*下線*/
        padding-left: 7px;
    }

    .private_work, .research_work, .team_work{
        width: 100%;
        max-width: 600px;
        height: 270px;
        border: 1px solid gray;

        @include mq(full){
            min-width: 600px;
            margin-right: 30px;
            margin-left: 30px;
        }

        @include mq(sp){
            width: 90%;
            height: 250px;

            // fullのmin-widthの打ち消し
            min-width: 300px;

            // margin-leftの打ち消しと中央揃え
            margin-left: initial;
            margin-left: auto;
            margin-right: auto;
        }

        background-color: #ABD9DE66;

        // 角を丸くする
        border-radius: 20px;

        // 子要素を上下左右中央揃え
        display: flex;
        flex-flow: column;
        margin-bottom: 25px;

        &:hover{
            // 浮き上がる
            box-shadow: 0 3px 6px 0 rgba(0, 0, 0, 0.25);
            transform: translateY(-0.1875em);
        }

        table{
            height: 70%;

            th{
                width: 25%;
                padding-left: 10px;
                &::after{
                    content: ":";
                }
            }

            td{
                padding-right: 10px;
            }

        }

    }

}

</style>

<script setup lang="ts">
import func from "~/vue-temp/vue-editor-bridge";


interface displayData{
    title: string;
    lang: string;
    library: string;
    framework: string;
    GitHubLink: string;
    description: string;
    MarkDownPath: string;
    displayOrder: number;
}

// search /content/works contents 
const { data:works } = await useAsyncData("works", () =>
queryContent("works").find())

// divide
let private_works: displayData[] = [];
let research_works: displayData[] = [];
let team_works: displayData[] = [];

// works内で値が入っている場所が_rawValue
works._rawValue.forEach((work) => {

    let addWork: displayData = {
        title: work.title, 
        lang: work.lang,
        library: work.library,
        framework: work.framework,
        GitHubLink: work.GitHub,
        description: work.description,
        MarkDownPath: work._path,
        displayOrder: work.display_order
    }

    if(addWork.MarkDownPath.includes('private')){
        private_works.push(addWork);
    }
    else if(addWork.MarkDownPath.includes("research")){

        if(work.page !== null){
            addWork.MarkDownPath = work.page
        }

        research_works.push(addWork);
    }
    else if(addWork.MarkDownPath.includes("team")){
        team_works.push(addWork);
    }

})

// sort by display_order
private_works.sort(
    function(a,b){
        if(a.displayOrder !== b.displayOrder){
            return a.displayOrder - b.displayOrder;
        }
        else{
            return a.title <= b.title;
        }

    }
);

</script>

<template>
    <div id="works_container">
        <div v-if="private_works.length !== 0" class="private_works_container">
            <h1>個人開発</h1>
            <div v-for="private_work in private_works" :key="private_work.MarkDownPath">
                <div class="private_work">
                    <!-- 本当ならここ以下は共通化したい -->
                    <h2>{{private_work.title}}</h2>
                    <table>
                        <tbody>
                            <tr v-if="private_work.lang"><th>言語</th><td>{{private_work.lang}}</td></tr>
                            <tr><th>概要</th><td>{{private_work.description}}</td></tr>
                            <tr><th>GitHub</th><td><a v-bind:href="private_work.GitHubLink">リポジトリを見る</a> </td></tr>
                            <tr v-if="private_work.library"><th>ライブラリ</th><td>{{private_work.library}}</td></tr>
                            <tr v-if="private_work.framework"><th>フレームワーク</th><td>{{private_work.framework}}</td></tr>
                            <tr><th>説明</th><td><nuxt-link :to="private_work.MarkDownPath">説明を確認する</nuxt-link></td></tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>

        <div v-if="research_works.length !== 0" class="research_works_container">
            <h3>研究関連の開発物</h3>
            <div v-for="research_work in research_works" :key="research_work.MarkDownPath">
                <div class="research_work">
                    <h2>{{research_work.title}}</h2>
                    <table>
                        <tbody>
                            <tr><th>言語</th><td>{{research_work.lang}}</td></tr>
                            <tr><th>概要</th><td>{{research_work.description}}</td></tr>
                            <tr><th>GitHub</th><td><a v-bind:href="research_work.GitHubLink">リポジトリを見る</a> </td></tr>
                            <tr><th>説明</th><td><nuxt-link :to="research_work.MarkDownPath">説明を確認する</nuxt-link></td></tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>

        <div v-if="team_works.length !== 0" class="team_works_container">
            <h1>チーム開発</h1>
            <div v-for="team_work in team_works" :key="team_work.MarkDownPath">
                <div class="team_work">
                    <h2>{{team_work.title}}</h2>
                    <table>
                        <tbody>
                            <tr><th>言語</th><td>{{team_work.lang}}</td></tr>
                            <tr><th>概要</th><td>{{team_work.description}}</td></tr>
                            <tr><th>GitHub</th><td><a v-bind:href="team_work.GitHubLink">リポジトリを見る</a> </td></tr>
                            <tr><th>説明</th><td><nuxt-link :to="team_work.MarkDownPath">説明を確認する</nuxt-link></td></tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</template>