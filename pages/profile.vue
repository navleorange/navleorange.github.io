<style lang="css">
body{
    /* font-family: "DotGothic16", sans-serif; */
    font-family: "M PLUS 1", sans-serif;
}
</style>

<style lang="scss" scoped>

#profile_container{
    // background-color: wheat;
    width: 100vw;
    height: 100vh;
}

#self_introduce{
    // background-color: aqua;
    width: auto;
    height: 50%;

    // 自己紹介を見やすい場所に変える
    margin-top: 8%;

    // iconとtextを横並びにする
    display: flex;
    justify-content: center;

    @include mq(sp){
        // スマホは縦
        display: block;

        // 自己紹介のサイズの調整
        height: 95%;
        margin-top: 15%;
    }

    #top_page_icon{

        // iconを円にする
        border-radius: 50%;
        object-fit: cover;

        @include mq(sp){
            // iconのサイズ (縦だと次の要素とぶつかるので横で指定)
            width: 75%;

            // 画像の左右中央揃え
            display: block;
            margin-left: auto;
            margin-right: auto;

            // self_introduceの上から少し離す
            position: relative;
            top: 5%;
        }

    }

    .profile_text{
        width: 30%;
        height: auto;

        // 文字中央
        text-align: center;

        // 枠線
        margin: 2em 0;
        margin-left: 3%;
        background: #f1f1f1;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.22);

        // text内の要素を左右中央揃え
        display: grid;
        place-items: center;

        @include mq(sp){
            width: 85%;
            height: 40%;

            // textの左右中央揃え
            display: block;
            margin-left: auto;
            margin-right: auto;

            margin-top: 13%;
        }

        // 紹介内容を列挙する前のタイトル
        .profile_title{
            font-size: 1.1em;
            background: #5fc2f5;
            padding: 4px;
            color:#F7FCFE;
            letter-spacing: 0.05em;

            // 縁取り
            text-shadow: 1px 1px 0 gray, -1px -1px 0 gray,
            -1px 1px 0 gray,  1px -1px 0 gray,
            1px 0 0 gray, -1px  0 0 gray,
            0 1px 0 gray,  0 -1px 0 gray;
        }

        .account{

            .account_link{
                // 外部のアカウントへのリンクの右寄せ
                text-align: right;

                img{
                    // iconを円にする
                    border-radius: 50%;
                    object-fit: cover;

                    // サイズ
                    width: 10%;

                    @include mq(sp){
                        margin-top: 3%;
                        margin-right: 10%;
                    }

                }
            }

            h2{
                margin-top: 0;

                @include mq(sp){
                    margin-top: -3%;
                }

            }
            
        }

    }

}

#my_skills{
    // 自己紹介より少し離す
    margin-top: 6%;
    height: 80%;
    // background-color: red;

    // h*を左右中央揃え
    text-align: center;

    @include mq(sp){
        // skillsのサイズ
        width: auto;

        // 左中中央揃え
        margin-right: auto;
        margin-left: auto;
    }

    table{
        // 左右中央揃え
        margin-left: auto;
        margin-right: auto;

        table-layout: fixed;
        width: 75%;
        height: 30%;

        // th, tdの間隔を設定
        border-collapse: collapse;

        background-color: aliceblue;

        @include mq(sp){
            font-size: 13px;
            height: 40%;
        }

        thead{
            background-color: yellowgreen;
            opacity: 0.7;
            border-bottom: 1px solid gray;
        }

        tbody{
            tr{
                border-bottom: 1px solid gray;

                &:hover{
                    background-color: wheat;
                    opacity: 0.7;
                }

                th td{
                    background-color: red;
                }
            }
        }

    }

}

</style>

<script>

export default{
    data(){
        return {
            age: 0,
            birthDay: {
                day:18,
                month: 7,
                dateTimeMonth: 7 - 1,
                year: 2001,
            },
        };
    },
    created(){
        this.InitAge();
    },
    methods:{
        InitAge(){
            let birthDayDate = new Date(this.birthDay.year, this.birthDay.dateTimeMonth);
            let today = new Date();
            this.age = today.getFullYear() - birthDayDate.getFullYear();
            let threshold_day = new Date(today.getFullYear(), this.birthDay.dateTimeMonth, this.birthDay.day);
            
            if(today < threshold_day){
                this.age--;
            }

        },
    }
}
</script>


<template>
    <div id="profile_container">
        <div id="self_introduce">
            <img id="top_page_icon" src="~/assets/image/icon.png"> 
            <div class="profile_text">
                <div class="profile_title">簡単な自己紹介</div>
                <div class="account">
                    <div class="account_link">
                        <a href="https://github.com/navleorange">
                        <img src="~/assets/image/github-mark.png">
                    </a>
                    </div>
                    <h2> アカウント: navleorange</h2>
                </div>
                <p>生年月日: {{birthDay.year}}年{{birthDay.month}}月{{birthDay.day}}日 {{age}}歳</p>
                <p>出身地: 静岡県</p>
                <p>サークル: <a href="https://szpp-dev-team.github.io">SZPP</a></p>
            </div>
        </div>

        <div id="my_skills">
            <h2>Works</h2>
            <table>
                <thead>
                    <tr><th>制作物・用途</th><th>使用したフレームワークや言語など</th><th>詳細</th></tr>
                </thead>
                <tbody>
                    <tr><th>研究</th><td>Python, Java</td><td><NuxtLink to='/research'>研究内容へ</NuxtLink></td></tr>
                    <tr><th>アルバイト</th><td>Laravel, PHP, MySQL</td><td><NuxtLink to='/job'>勤務先情報</NuxtLink></td></tr>
                    <tr><th>競技プログラミング</th><td>C++</td><td><a href="https://atcoder.jp/users/navleorange">AtCoderリンク</a></td></tr>
                    <tr><th>このポートフォリオ</th><td>Nuxt3, Vue.js, GitHub Actions</td><td><NuxtLink to='/works'>制作物へ</NuxtLink></td></tr>
                    <tr><th>その他の制作物</th><td>Python, Goなど</td><td><NuxtLink to='/works'>制作物へ</NuxtLink></td></tr>
                    <tr><th>その他</th><td></td><td><NuxtLink to='/works/other'>その他へ</NuxtLink></td></tr>
                </tbody>
            </table>
        </div>

    </div>
</template>